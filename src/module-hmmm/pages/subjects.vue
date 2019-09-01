<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        
        <el-form style="margin-top:10px">
          <el-form-item>
            <el-button @click="addSubjects">新增学科</el-button>
          </el-form-item>
          
          <el-row type="flex" justify="space-center">
            <el-form-item style="margin-right: 100px" label-width="80px" label="学科名称:">
              <el-input style="width:200px"></el-input>
            </el-form-item>

            <el-form-item>
              <el-button>清除</el-button>
              <el-button type="primary">搜索</el-button>
            </el-form-item>
          </el-row>
        </el-form>

        <el-table :data="dataList"  border>
          <el-table-column align="center" prop="id" label="序号" ></el-table-column>
          <el-table-column align="center" prop="subjectName" label="学科名称" ></el-table-column>
          <el-table-column align="center" prop="username" label="创建者"></el-table-column>
          <el-table-column align="center" prop="addDate" label="创建日期"></el-table-column>
          <el-table-column align="center" prop="isFrontDisplay" :formatter="isFrontDisplayFormatter" label="前台是否显示"></el-table-column>
          <el-table-column align="center" prop="twoLevelDirectory" label="二级目录"></el-table-column>
          <el-table-column align="center" prop="tags" label="标签"></el-table-column>
          <el-table-column align="center" prop="totals" label="题目数量"></el-table-column>
          <el-table-column align="center"  label="操作">
            <el-button type="text">学科分类</el-button><br>
            <el-button type="text" @click="subjectTags">学科标签</el-button><br>
            <el-button type="text">修改</el-button><br>
            <el-button type="text">删除</el-button>
          </el-table-column>
        </el-table>
      </el-card>
    </div>
  </div>
</template>

<script>

import { detail, simple, list } from '@/api/hmmm/subjects.js'

export default {
  name: 'SubjectsList',
  data() {
    return {
      dataList: []
    }
  },
  methods: {
    // 初始化数据
    initialDate() {
      this.getList()
    },

    // 获取列表数据
    async getList() {
      const { data: res } = await list()
      this.dataList = res.items
      this.dataList = this.dataList.reverse()
      console.log(this.dataList)
    },

    // 是否前台显示
    isFrontDisplayFormatter(row, column, cellValue, index) {
      return cellValue ? '是' : '否'
    },
    
    // 学科标签的点击事件
    subjectTags() {
      this.$router.push('/subjects/tags')
    },

    // 新增学科事件
    addSubjects() {
      
    }
  },

  created() {
    this.initialDate()
  }
}
</script>

<style scoped>
</style>
