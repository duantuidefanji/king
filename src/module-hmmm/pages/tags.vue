<template>
  <el-card>
    <el-breadcrumb
      separator=">"
      slot="header"
    >
      <el-breadcrumb-item :to="{ path: '/subjects/list' }"><i style="color:red">学科管理</i> </el-breadcrumb-item>
      <el-breadcrumb-item>javaEE</el-breadcrumb-item>
      <el-breadcrumb-item>标签管理</el-breadcrumb-item>
    </el-breadcrumb>
    <el-row style="margin-top:10px">
      <el-button
        type="success"
        @click="dialogFormVisible=true,flag=true"
      >新建标签</el-button>
      <el-button type="info">返回学科</el-button>
    </el-row>
    <el-row
      type="flex"
      justify="space-between"
      style="margin-top:20px"
    >
      <div class="left">
        <el-tag>标签名称</el-tag>
        <el-input
          placeholder="请输入"
          style="width:200px"
        ></el-input>
      </div>
      <div class="right">
        <el-button
          type="primary"
        >清除</el-button>
        <el-button
          type="success"
        >搜索</el-button>
      </div>
    </el-row>
    <!-- 动态生成表格 -->
    <el-table
    :data="dataList"
    style="width: 100%">
    <el-table-column label="序号" width="80">
      <template slot-scope="scope">{{scope.$index+1}}</template>
    </el-table-column>
    <el-table-column
      prop="tagName"
      label="标签名称"
      width="180">
    </el-table-column>
    <el-table-column
      prop="username"
      label="创建者">
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
<!-- 设置分页界面 -->
<el-row justify="center" type="flex">
<el-pagination
 background
 @current-change="handleCurrentChange"
 :current-page="Number(requestParameters.page)"
 :total="Number(total)"
 :page-size="Number(requestParameters.pagesize)"
 :page-sizes="[10,20,30, 50]"
 layout="sizes, prev, pager, next, jumper"
 ></el-pagination>
</el-row>
  </el-card>
</template>

<script>
import {list, detail, add, updata, remove, removeState} from '@/api/hmmm/tags'
export default {
  name: 'TagsList',
  data() {
    return {
      dialogFormVisible: false,
      flag: true,
      total: 0,
      requestParameters: {
        page: 1,
        pagesize: 10
      },
      dataList: []
    }
  },
  methods: {
    // 获取列表数据
   async getList() {
      const { data: res } = await list(this.requestParameters)
      // 获取数据给dataList
      this.dataList = res.items
      console.log(this.dataList)
      // 获取总条数
      this.total = res.counts
    },
    handleCurrentChange(newPage) {
    }
  },
  created() {
    this.getList()
  }, 
filters: {
  transformName(value) {
  return value ? '开启' : '关闭'
  }
}
  
}
</script>

<style scoped>
</style>
