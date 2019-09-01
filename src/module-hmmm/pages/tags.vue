<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-form>
          <el-form-item type="flex">
            <el-button>新建标签</el-button>
            <el-button @click="backSubjects">返回学科</el-button>
          </el-form-item>

          <el-form-item label="标签名称：" label-width="100px">
            <el-input placeholder="请输入" style="width:200px"></el-input>
          </el-form-item>

          <el-table :data="dataList" border>
            <el-table-column align="center" prop="id" label="序号">
              <template slot-scope="scope">
                <span>{{ scope.row.id }}</span>
              </template>
            </el-table-column>
            <el-table-column align="center" label="标签名称">
              <template slot-scope="scope">
                <span>{{ scope.row.subjectName }}</span>
              </template>
            </el-table-column>
            <el-table-column align="center" label="创建者">
              <template slot-scope="scope">
                <span>{{ scope.row.username }}</span>
              </template>
            </el-table-column>
            <el-table-column align="center" label="创建日期">
              <template slot-scope="scope">
                <span>{{ scope.row.addDate}}</span>
              </template>
            </el-table-column>
            <el-table-column align="center" label="创建面试题数量">
              <template slot-scope="scope">
                <span>{{ scope.row.totals}}</span>
              </template>
            </el-table-column>
            <el-table-column align="center" label="状态">
              <template slot-scope="scope">
                <span>{{ scope.row.state | stateFilters }}</span>
              </template>
            </el-table-column>
            <el-table-column align="center" label="操作">
              <template slot-scope="scope">
                <el-button type="text" style="width:30;">修改</el-button>
                <el-button type="text" style="width:30px;" @click="isNotDisabled(scope.row)">禁用</el-button>
                <el-button type="text" style="width:30px;">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
import { list, detail, removeState } from '@/api/hmmm/tags.js'
export default {
  name: 'TagsList',
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

    // 标签是否禁用
    isNotDisabled(val) {
      var status = ''
     
      if (val.state) {
        status = '禁用'
        val.state = 0
      } else {
        status = '开启'
        val.state = 1
      }
      var data = {
        id: val.id,
        state: val.state
      }
      this.$confirm('已成功' + status + ',是否继续？', '提示', {
        type: 'warning'
      }).then(async () => {
        await removeState(data).then(response => {
          this.$message.success('已成功' + status + '!')
          this.getList()
        }).catch(response => {
          this.$message.error(status + '失败')
        })
      })
    },

    // 返回学科列表
    backSubjects() {
      this.$router.push('/subjects/list')
    }
  },
  filters: {
    stateFilters(value) {
      return value ? '是' : '否'
    }
  },
  async created() {
    this.initialDate()
  }
}
</script>

<style scoped>
</style>
