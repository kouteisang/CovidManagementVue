<template>
  <div>
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-lx-calendar"></i> 公告管理专区
        </el-breadcrumb-item>
        <el-breadcrumb-item>发布公告</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="container">
      <div class="form-box">
        <el-form ref="form" :model="form" :rules="rules"  label-width="100px">
          <el-form-item label="公告标题" prop="newsTitle">
            <el-input v-model="form.newsTitle" placeholder="请输入公告标题" style="width:400px"/>
          </el-form-item>
          <el-form-item label="公告内容" prop="newsContent">
            <el-input v-model="form.newsContent" type="textarea" :rows="10" placeholder="请输入公告内容"/>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSubmit">添加公告</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import qs from "qs";
let header = { 'content-type': 'application/x-www-form-urlencoded' }
export default {
  name: "AddNews",
  data(){
    return{
      operatorId: localStorage.getItem("ms_username"),
      form:{
        newsTitle:'',
        newsContent:''
      },
      rules:{
        newsTitle: [
          { required: true, message: '请输入身份证号', trigger: 'blur' },
          { min: 5, message: '标题最少五个字符进行描述', trigger: 'blur' }
        ],
        newsContent: [
          { required: true, message: '请输入公告内容', trigger: 'blur' }
        ]
      }
    }
  },methods:{
    onSubmit(){
      let url = 'http://localhost:8181/manager/news/addNews'
      const that = this
      let params = {
        newsTitle: this.form.newsTitle,
        newsContent: this.form.newsContent,
        operator: this.operatorId
      }
      axios.post(url, qs.stringify(params), {headers:header}).then(function (resp){
        if(resp.data.code == "600"){
          that.$message({
            message: '您无权限操作，请先进行实名认证！',
            type:'warning'
          })
        }
        else if(resp.data.code == "200") {
          that.$message({
            message: '发布公告成功',
            type: 'success'
          });
        }else if(resp.data.code != "200"){
          that.$message({
            message: '发布公告失败，请输入完整信息！',
            type: 'error'
          });
        }
      })


    }
  }
}
</script>

<style scoped>

</style>