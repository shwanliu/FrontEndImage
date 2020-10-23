<template>
  <div align="center">
    <!-- <el-card style="height:460px"> -->
    <el-row>
      <el-col :span="15">
        <div style="margin:20px;">
          <img v-if="imageUrl" :src="imageUrl" width="300" class="avatar" />
          <!-- </div> -->
          <el-upload
            style="margin-top:30px;width:100%"
            drag
            action="/123"
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
            <!-- <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div> -->
          </el-upload>
        </div>
      </el-col>
      <el-col span="9">
        <div style="margin:20px;">
          <el-card class="box-card">
            <div v-if="!feature">暂无属性</div>
            <div
              align="left"
              v-for="(score,feat) in feature"
              style="list-style-type: none;"
              :key="feat"
            >{{feat+':'+score}}</div>
          </el-card>
          <!-- <div class="grid-content bg-purple-light"> -->
          <!-- <el-card v-if='data'>
                <li v-for="(score,feature) in data" style="list-style-type: none;" :key="feature">{{feature+':'+score}}</li>
          </el-card>-->
        </div>
      </el-col>
    </el-row>
    <!-- </el-card> -->
    <!-- <img v-if="imageUrl" :src="imageUrl" width="320" class="avatar" /> -->
    <!-- <el-upload
      style="margin-top:30px;width:100%"
      drag
      action="/123"
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
    <!-- <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div>-->
    <!-- </el-upload> -->
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      imageUrl: require("@/assets/noUpLoad.png"),
      // imageUrl:""
      feature: "",
    };
  },
  methods: {
    beforeUpload(file) {
      // this.file = file;
      let fd = new FormData();
      fd.append("image", file); //传文件
      this.imagePreview(file);
      axios.post("/faceAttr", fd).then((resp) => {
        this.feature = resp.data.data;
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