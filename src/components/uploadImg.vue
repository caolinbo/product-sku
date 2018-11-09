<template>

  <el-upload
    class="avatar-uploader"
    :class="{avatarUploaderMimi:size == 'mini', disabled:fileList.length >= (imageCount?imageCount:1)}"
    :action="uploadUrl"
    :file-list="fileList"
    name="file"
    list-type="picture-card"
    :on-change="handleChange"
    :on-remove="handleRemove">
    <div class="avatar-uploader-center">
      <i class="el-icon-plus avatar-uploader-icon"></i>
    
      <div class="el-upload__tip" v-show="text">{{text}}</div>
    </div>
    
  </el-upload>

</template>　

<script>

/**
 * @param imageUrl : [{url:''}]
 * @param imageCount : 上传数量
 * @param text 
 * @returns arr
 */

export default {
  name: 'uploadImg',

  props: ['imageUrl', 'imageCount', 'text' , 'size'],

  data() {
    return {
      uploadUrl:'',
      fileList:[],
    };
  },
  
  created() {

    this.uploadUrl = "";
    if(this.imageUrl && this.imageUrl.length > 0){
      this.fileList = this.imageUrl;
    }
   
  },

  methods: {

    handleChange(file, fileList) {
      this.fileList = fileList;
    },


    handleRemove(file, fileList) {
      
      this.fileList = fileList;
    },
 
    beforeAvatarUpload(file) {

      const isJPEG = file.type === 'image/jpeg';
      const isJPG = file.type === 'image/jpg';
      const isPNG = file.type === 'image/png';
      const isGIF = file.type === 'image/gif';
      const isLt2M = file.size / 1024  < 512;

      if (!isLt2M) {
        this.$message.error('上传图片大小不能超过512k!');
      }
       return (isJPG || isJPEG || isPNG || isGIF) && isLt2M;
    },


    // 返回图片在服务器的URL
    getUploadImage(){
      let imgUrl;
      this.fileList.map(item => {
        if(item.response){
          imgUrl = item.response.data;
        }else {
          imgUrl = item.url;
        }
      });
      return imgUrl;
    },


  }

}

</script>

<style>

  .avatar-uploader .el-upload {
    line-height: inherit;
  }

  .avatar-uploader .el-icon-close-tip{
    opacity: 0;
  }

  .avatar-uploader .el-upload,
  .avatar-uploader .el-upload-list__item{
    width: 100px !important;;
    height: 100px !important;;
  }

  .avatarUploaderMimi .el-upload,
  .avatarUploaderMimi .el-upload-list__item{
    width: 50px !important;;
    height: 50px !important;;
  }

  
  .avatar-uploader.disabled .el-upload--picture-card {
    display: none;
  }

  .avatar-uploader-center{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    height: 100%;
  }

</style>
