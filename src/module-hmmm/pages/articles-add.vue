<template>
  <el-card>
    <el-form :model="formData" :rules="rules" ref="loginForm">
      <el-form-item prop="title">
        <p>
          <label for="view-add-ipt">标题:</label>
          <el-input
            v-model="formData.title"
            placeholder="请输入内容"
            class="view-add-ipt"
            id="view-add-ipt"
          ></el-input>
        </p>
      </el-form-item>
      <!-- <el-form-item prop="articleBody" label="内容">
        <quill-editor
          style="width:700px;height:400px;margin-bottom:100px"
          v-model="formData.articleBody"
          type="textarea"
          :rows="4"
          placeholder="请输入内容"
      ></quill-editor>-->
      <el-form-item prop="articleBody">
        <quill-editor
          style="width:800px;height:218px;margin-bottom:100px"
          type="textarea"
          :rows="4"
          placeholder="请输入内容"
          v-model="formData.articleBody"
        ></quill-editor>
      </el-form-item>
      <el-form-item prop="videoURL">
        <label for="view-add-ipt2">视频地址:</label>
        <el-input class="view-add-ipt" id="view-add-ipt2" v-model="formData.videoURL"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="view_add(formData)">提交</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>
  </el-card>
</template>

<script>
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'
import { add } from '@/api/hmmm/articles'
export default {
  name: 'articles-add',
  data() {
    return {
      formData: {
        title: '',
        articleBody: '',
        videoURL: ''
      },
      rules: {
        title: [{ required: true, message: '请输入内容', trigger: 'blur' }],
        arvideoURL: [
          { required: true, message: '请选择活动区域', trigger: 'blur' },
          { min: 3, max: 15, message: '长度在 3 到 15 个字符', trigger: 'blur' }
        ],
        videoURL: [
          {
            required: true,
            message: '内容不能为空',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    view_add(formData) {
      this.$refs.loginForm.validate((isOk, result) => {
        // console.log(isOk)
        // console.log(result)
        if (isOk) {
          add(formData).then(() => {
            this.$router.push('/articles/list')
          })
          // this.$axios({
          //   method: 'post',
          //   url: '/authorizations',
          //   data: this.formData
          // })
          //   .then(result => {
          //     window.localStorage.setItem(
          //       'user-info',
          //       JSON.stringify(result.data)
          //     )
          //     this.$router.push('/home')
          //   })
          //   .catch(() => {
          //     this.$message({
          //       message: '用户名或密码错误,请重新输入',
          //       type: 'warning'
          //     })
          //   })
        }
      })
    }
  }
}
</script>

<style>
.view-add-ipt {
  width: 700px;
}
</style>
