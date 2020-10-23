<template>
  <div align="center">
    <!-- <div class="grid-content bg-purple"> -->
    <el-card style="height:460px">
      <img v-if="imageUrl" :src="imageUrl" width="320" class="avatar" />
       <span>{{resUrl}}</span>
    </el-card>
    <el-upload
     style="margin-top:30px;width:100%"
      drag
      action="http://localhost:18080/123"
      :before-upload="beforeUpload"
      :show-file-list="false"
      accept="image/jpeg, image/gif, image/png, image/bmp"
      multiple
    >
      <i class="el-icon-upload"></i>
      <div class="el-upload__text">
        将文件拖到此处，或
        <em>点击上传</em>
      </div>
       <!-- <el-button type="primary" @click="beforeUpload">
          开始识别
        </el-button> -->
      <!-- <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div> -->
    </el-upload>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HumanPose',
  data() {
    return {
      imageUrl: require("@/assets/dance_foot.gif"),
      resUrl: "",
    };
  },
  methods: {
    beforeUpload(file) {
      console.log(file)
      // this.file = file;
      let fd = new FormData();
      fd.append("image", file); //传文件
      this.imagePreview(file);
    //   getPose(fd).then((resp) => {
    //     // resp = resp.json()

    //     console.log(resp.data)
    //     this.imageUrl= resp.data.data;
    //     this.resUrl = resp.data.data
    //     console.log(this.resUrl)
    //   });
    //   fd.append('srid',this.aqForm.srid);//传其他参数
      axios.post("/getPose", fd).then((resp) => {
        // resp = resp.json()
        this.imageUrl= resp.data.data;
        console.log(this.resUrl)
      });


    }, //当上传图片后，调用onchange方法，获取图片本地路径
    imagePreview: function (file) {
      var self = this;
      //定义一个文件阅读器
      var reader = new FileReader();
      //文件装载后将其显示在图片预览里
      reader.onload = function (e) {
        //将bade64位图片保存至数组里供上面图片显示
        self.imageUrl = e.target.result;
      };
      reader.readAsDataURL(file);
    },
  },
};
</script>

<style>
</style>