<template>
  <div align="center">
    <!-- <el-card style="height:230px"> -->
      <el-row>
        <el-col style="height:50px"></el-col>
        <el-col style="height:300px" :span="12">
          <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>原图像</span>
          </div>
          <el-upload
            action="/123"
            id="src"
            :limit="1"
            list-type="picture-card"
            :before-upload="src_beforeUpload"
            :on-preview="src_handlePictureCardPreview"
            :on-remove="handleRemove"
          >
            <i class="el-icon-plus"></i>
          </el-upload>
            <el-dialog :visible.sync="src_dialogVisible">
            <img width="80%" :src="srcUrl" alt="">
          </el-dialog>
          </el-card>
        </el-col>
        <el-col :span="12">
          <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>目标图像</span>
          </div>
          <el-upload
            action="/123"
            id="dst"
            :limit="1"
            list-type="picture-card"
            :before-upload="dst_beforeUpload"
            :on-preview="dst_handlePictureCardPreview"
            :on-remove="handleRemove"
          >
            <i class="el-icon-plus"></i>
          </el-upload>
          <el-dialog :visible.sync="dst_dialogVisible">
            <img width="80%" :src="dstUrl" alt="">
          </el-dialog>
            </el-card>
        </el-col>
        <!-- <el-col  style="margin-top:10px;" :span="24">
            <el-button type="primary" @click="faceSwap" >开始换脸</el-button>
        </el-col> -->
      </el-row>
    <!-- </el-card> -->
    <!-- <el-card style="height:400px"> -->
      <!-- <el-dialog :visible.sync="dst_dialogVisible"> -->
      <img style="margin-top:2px" height="320px" :src="resUrl" alt />
      <!-- </el-dialog> -->
        <el-col  style="margin-top:10px;" :span="24">
            <el-button type="primary" @click="faceSwap" >开始换脸</el-button>
        </el-col>
    <!-- </el-card> -->
    <!-- <img v-if="imageUrl" :src="imageUrl" width="300" class="avatar" /> -->
    <!-- <img v-if="imageUrl" :src="imageUrl" width="320" class="avatar" /> -->
        <!--拍照-->
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      srcUrl: "",
      dstUrl: "",
      resUrl: require("@/assets/noUpLoad.png"),
      src_dialogVisible: false,
      dst_dialogVisible: false,
      fd: new FormData(),
    };
  },
  mounted() {
    // this.memberInit = Object.assign({}, this.member)
    // 摄像头
    this.canvas = document.getElementById('canvas')
    this.video = document.getElementById('video')
    // this.setHeaders() // 上传token
  },
  methods: {
    src_beforeUpload(file) {
      this.fd.append("srcImage", file); //传文件
    },
    dst_beforeUpload(file) {
      this.fd.append("dstImage", file); //传文件
    },

    faceSwap() {
      console.log(this.fd);
      if(this.fd["srcImage"]!=null && this.fd["dstImage"]!=null){
        alert("请选择原图像和目标图像！")
      }else{
        axios.post("/faceSwap", this.fd).then((resp) => {
        this.resUrl = resp.data.data;
      });
      }
    },
    handleRemove(file, fileList) {
      //  this.fd = ""
      this.fd = new FormData()
      this.resUrl=require("@/assets/noUpLoad.png"),
      console.log(file, fileList);
    },
    dst_handlePictureCardPreview(file) {
      console.log(file);
      this.dstUrl = file.url;
      this.dst_dialogVisible = true;
    },
    src_handlePictureCardPreview(file) {
      console.log(file);
      this.srcUrl = file.url;
      this.src_dialogVisible = true;
    }
  },
};
</script>

<style>
.text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 18px;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 480px;
  }
</style>