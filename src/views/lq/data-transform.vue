<template>
  <div class="mod-demo-ueditor">

    <el-upload action="" :http-request="uploadFile" accept=".xls,.xlsx" :before-upload="onBeforeUpload">
      <el-button size="small" type="primary">点击上传</el-button>
      <div slot="tip" class="el-upload__tip">请上传.xls|.xlsx格式文件</div>
    </el-upload>

    <a ref="downloadA" style="display: none" class='download' :href='downloadUrl' title="下载">下载</a>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        ue: null,
        ueContent: '',
        dialogVisible: false,
        downloadUrl: this.$http.adornUrl('/lq/dt/download')
      }
    },
    methods: {
      uploadFile (item) {
        const form = new FormData()
        form.append('file', item.file)
        this.$http({
          url: this.$http.adornUrl('/lq/dt/dateTransform'),
          method: 'post',
          data: form
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.$refs['downloadA'].href = this.$http.adornUrl('/lq/dt/download') + '?token=' + this.$cookie.get('token')
            this.$refs['downloadA'].click()

            this.$message({
              message: '操作成功',
              type: 'success',
              duration: 1500,
              onClose: () => {
                this.visible = false
              }
            })
          } else {
            this.$message.error(data.msg)
          }
        })
      },
      onBeforeUpload (file) {
        return true
        // const isIMAGE = (file.type === 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' || file.type === 'application/vnd.ms-excel')
        // // const isLt1M = file.size / 1024 / 1024 < 1;
        //
        // if (!isIMAGE) {
        //   this.$message.error('上传文件只能是.xls|.xlsx格式!')
        // }
        // // if (!isLt1M) {
        // //   this.$message.error('上传文件大小不能超过 1MB!');
        // // }
        // return isIMAGE// && isLt1M;
      }
    }
  }
</script>

<style lang="scss">
  .mod-demo-ueditor {
    position: relative;
    z-index: 510;
    > .el-alert {
      margin-bottom: 10px;
    }
  }
</style>
