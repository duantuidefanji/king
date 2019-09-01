<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-table :data="dataList">
          <el-table-column prop="id" align="center" label="序号"></el-table-column>
          <el-table-column prop="addTime" align="center" label="组题时间"></el-table-column>
          <el-table-column prop="userName" align="center" label="用户名"></el-table-column>
          <el-table-column prop="questionIDs[0]" align="center" label="试题ID"></el-table-column>
          <el-table-column prop="progressOfAnswer" align="center" label="答题进度"></el-table-column>
          <el-table-column prop="accuracyRate" align="center" label="正确率"></el-table-column>
          <el-table-column prop="totalSeconds" align="center" label="答题总耗时（秒）" width="150px"></el-table-column>
          <el-table-column prop="questionType" align="center" label="组题类型/详情"></el-table-column>
          <el-table-column align="center" label="操作"></el-table-column>
        </el-table>
      </el-card>
    </div>
  </div>
</template>

<script>
import { randoms } from '@/api/hmmm/questions.js'
export default {
  name: 'QuestionsRandoms',
  data() {
    return {
      dataList: [],
      count: 0
    }
  },
  methods: {
    // 初始化数据
    initialData() {
      this.getList()
    },
    async getList() {
      const {data: res} = await randoms()
      this.dataList = res.items
      console.log(this.dataList)
    }
  },
  created() {
    this.initialData()
  }
}
</script>

<style scoped>
</style>
