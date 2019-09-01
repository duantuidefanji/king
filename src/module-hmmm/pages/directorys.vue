<template>
  <el-card>
<el-breadcrumb separator=">" slot="header">
  <el-breadcrumb-item :to="{ path: '/subjects/list' }"><i style="color:red">学科管理</i> </el-breadcrumb-item>
  <el-breadcrumb-item>javaEE</el-breadcrumb-item>
  <el-breadcrumb-item>分类管理</el-breadcrumb-item>
</el-breadcrumb>
<el-row style="margin-top:10px">
  <el-button type="success" @click="dialogFormVisible=true,flag=true">新建目录</el-button>
  <el-button type="info">返回学科</el-button>
</el-row>
<!-- dialog 弹出框 用于新建标签 -->
<el-dialog title="目录" :visible.sync="dialogFormVisible">
  <el-form style="margin-top:30px">
    <el-form-item label="目录名称" label-width="80px">
      <el-input autocomplete="off" style="width:300px" v-model='formData.directoryName' placeholder="请输入"></el-input>
    </el-form-item>
  </el-form>
     <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false,this.formData.directoryName = ''">取 消</el-button>
    <el-button type="primary" @click="orDirectory">确 定</el-button>
  </div>
</el-dialog>
<el-form :inline="true"  :model="formData" :rules="rules" ref="ruleForm"  class="demo-form-inline" style="margin-top:30px">
  <el-form-item label="目录名称" prop="directoryName">
    <el-input v-model="formData.directoryName" placeholder="请输入"></el-input>
  </el-form-item >
  <el-form-item label="状态"  prop="state">
    <el-select v-model="formData.state" placeholder="请选择" >
    <el-option
      v-for="item in status"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
  </el-select>
  </el-form-item>
  <el-form-item>
     <el-button type="info" @click="clear">清除</el-button>
    <el-button type="primary" @click="search">查询</el-button>
  </el-form-item>
</el-form>
<!-- 动态生成表格 -->
 <el-table
    :data="list"
    style="width: 100%">
    <el-table-column label="序号" width="80">
      <template slot-scope="scope">{{scope.row.id}}</template>
    </el-table-column>
    <el-table-column
      prop="directoryName"
      label="目录名称"
      width="180">
    </el-table-column>
    <el-table-column
      prop="creator"
      label="创建者">
      派大星
    </el-table-column>
    <el-table-column label="创建日期">
      <template slot-scope="scope">{{scope.row.addDate|parseTimeByString()}}</template>
    </el-table-column>
     <el-table-column
      prop="totals"
      label="面试题数量">
    </el-table-column>
    <el-table-column label="状态">
       <template slot-scope="scope">{{scope.row.state|transformName}}</template>
    </el-table-column>
    <el-table-column label="操作" width='200'>
      <template slot-scope="scope">
    <el-button type="text" @click="alert(scope.row)">修改</el-button>
    <el-button type="text" @click="forbid(scope.row)">禁用</el-button>
    <el-button type="text" @click="delItem(scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <el-row style="margin-top:30px" type="flex" justify="center">
   <el-pagination
  background
  @current-change='changePage'
  :current-page="page.page"
  :page-size="page.pagesize"
  :total="page.total">
</el-pagination>
  </el-row>
  </el-card>
</template>

<script>
import {status} from '@/api/hmmm/constants'
import {list, detail, add, update, remove, removeState} from '@/api/hmmm/directorys'
import { constants } from 'fs'
export default {
 name: 'DirectorysList',
 data() {
   return {
   status,
   dialogFormVisible: false,
   flag: true,
   formData: {
     state: '', // 目录状态
     id: '', // 数据Id
     directoryName: '', // 目录名称
     subjectID: '' // 学科id
   },
   page: {
     page: 1,
     pagesize: 10,
     total: 0
   },
   list: [],
   rules: {
         directoryName: [
            { required: true, message: '请输入学科名称', trigger: 'blur' }
          ],
          state: [
            {required: true, message: '请选择学科状态', trigger: 'blur'}
            ]
   }
 }
 },
 methods: {
async getDatas() { // 获取页面数据
 let result = await list(this.page)
 this.list = result.data.items
 this.page.total = result.data.counts
 },
  search() { // 目录搜索 查询数据
  this.$refs.ruleForm.validate(async isOk => {
    if (isOk) { 
    let result = await list({...this.page, ...this.formData})
    this.list = result.data.items
    this.page.total = result.data.counts 
}
  })
   },
 async clear() { // 搜索记录删除
  this.formData.directoryName = ''
   this.formData.state = ''
   await this.getDatas()
 },
 alert(data) { // 点击修改按钮  弹出框
  this.dialogFormVisible = true
  this.flag = false
   this.formData.id = data.id
   this.formData.directoryName = data.directoryName
 },
 async orDirectory() { // 弹出框  进行目录的添加/修改操作
 this.flag ? await add(this.formData) : await update(this.formData)
 this.dialogFormVisible = false
 this.formData.directoryName = ''
 this.getDatas()
 this.$message({
          message: `恭喜你,${this.flag ? '目录添加成功' : '目录修改成功'}`,
          type: 'success'
        })
 },
 async forbid(data) { // 状态切换
 data.state = Number(!data.state)
 await removeState(data)
  this.getDatas()
 },
 async delItem(data) {
   this.formData.id = data.id
   await this.$confirm('您确定要删除吗?', '提示')
   await remove(this.formData)
    this.getDatas()
 },
  changePage(newPage) {
   this.page.page = newPage
    this.getDatas()
 }
 },
created() {
  this.getDatas(this.page)
  this.formData.subjectID = Number(this.$route.params.id)
  
},
filters: {
  transformName(value) {
  return value ? '开启' : '关闭'
  }
}
}
</script>

<style>

</style>
