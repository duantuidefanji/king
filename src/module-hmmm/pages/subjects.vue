<template>
  <el-card>
    <template slot="header">
      <el-button @click="addFormVisible=true">新增学科</el-button>
    </template>
    <el-dialog title="新增学科" :visible.sync="addFormVisible">
  <el-form  label-width="80px">
    <el-form-item label="学科名称" >
      <el-input autocomplete="off" v-model="newSubject"></el-input>
    </el-form-item>
     <el-form-item label="学科状态">
     <el-switch
  v-model="visible"
  active-text='前台显示'
  active-value="1"
  inactive-text='前台隐藏'
  inactive-value="0"
  active-color="#13ce66"
  inactive-color="#ff4949">
</el-switch>
     </el-form-item>
  </el-form>
   <div slot="footer" class="dialog-footer">
    <el-button @click="addFormVisible=false">取 消</el-button>
    <el-button type="primary" @click="addSubject">确 定</el-button>
  </div>
    </el-dialog>
    <el-row type="flex" justify="space-between">
      <div class="left">
      <el-tag>学科名称</el-tag>
      <el-input placeholder="请输入" style="width:200px" v-model="subjectName"></el-input>
      </div>
    <div class="right">
      <el-button type="primary" @click="clearItem">清除</el-button>
       <el-button type="success" @click="searchItem">搜索</el-button>
    </div>
    </el-row>
     <el-table
    :data="tableData"
    style="width: 100%">
    <el-table-column
      label="序号"
      width="80">
      <template slot-scope="scope">{{scope.row.id}}</template>
    </el-table-column>
    <el-table-column
      prop="subjectName"
      label="学科名称"
      width="180">
    </el-table-column>
    <el-table-column
      label="创建日期">
      <template slot-scope="scope">{{ scope.row.addDate|parseTimeByString('{y}-{m}-{d}') }}</template>
    </el-table-column>
     <el-table-column
      label='前台能否显示'>
      <template slot-scope="scope">{{ scope.row.isFrontDisplay }}</template>
    </el-table-column>
     <el-table-column
      prop="twoLevelDirectory"
      label="二级目录">
    </el-table-column>
     <el-table-column
      prop="tags"
      label="标签">
    </el-table-column>
     <el-table-column
      prop="totals"
      label="题目数量">
    </el-table-column>
     <el-table-column
      label="操作"  width="160">
       <template slot-scope="scope">
       <el-button type="text" @click="switchOver(scope.row)">学科分类</el-button>
        <el-button type="text" @click="subjectObj(scope.row)">学科标签</el-button>
        <el-button type="text"  @click="alertData(scope.row)">修改</el-button>
         <el-button type="text" @click="delData(scope.row)">删除</el-button>
       </template>
    </el-table-column>
  </el-table>
  <el-dialog title="学科内容修改" :visible.sync="dialogFormVisible">
      <el-tag>学科名称</el-tag>
      <el-input placeholder="请输入" style="width:200px" v-model="currentName"></el-input>
    <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false">取 消</el-button>
    <el-button type="primary" @click="submitSure">确 定</el-button>
    </div>
    </el-dialog>
  <el-row style="margin-top:40px" type="flex" justify="center"> 
    <el-pagination
  background
  :current-page="page.page"
  :page-size="page.pagesize"
  :total="page.total"
  @current-change='selectPage'>
</el-pagination>
  </el-row>
  </el-card>
</template>

<script>
import {list, remove, update, add} from '@/api/hmmm/subjects'
export default {
  name: 'SubjectsList',
  data() {
    return {
      newSubject: '',
      subjectName: '',
      currentId: '',
      visible: 0,
      addFormVisible: false,
      dialogFormVisible: false,
      tableData: '',
      currentName: '',
      page: {
      page: 1,
      pagesize: 10,
      total: 0
    }
    }
  },
  methods: {
    // 添加学科
    async addSubject() {
    let params = {subjectName: this.newSubject, isFrontDisplay: this.visible}
     await add(params)
      this.getSubjectMess()
      this.addFormVisible = false
      await this.$message({
          message: '恭喜你，学科添加成功',
          type: 'success'
        })
    },
    // 删除
    async delData(data) {
     await this.$confirm('此操作将永久删除该文件, 是否继续?', '提示')
     await remove(data)
     this.getSubjectMess()
    },
   // 修改
   alertData(data) {
    this.dialogFormVisible = true
    this.currentName = data.subjectName
    this.currentId = data.id
   },
  //  弹出框  确认修改
   async submitSure() {
   let params = {
     id: this.currentId,
     subjectName: this.currentName
   }
   await update(params)
    this.getSubjectMess()
   this.dialogFormVisible = false
   },
    // 学科标签页面切换
    subjectObj(data) {
      this.$router.push(`/subjects/directorys/${data.id}`)
    },
    // 学科目录页面切换
    switchOver(data) {
      this.$router.push(`/subjects/tags/${data.id}`)
    },
    // 搜索
    searchItem() {
      this.page.page = 1
       this.page.subjectName = this.subjectName 
      this.getSubjectMess(this.page)
    },
    // 清除搜索数据
    clearItem() {  
      this.subjectName = ''
      this.getSubjectMess()
    },
    // 分页选择
    selectPage(newPage) {  
      this.page.page = newPage 
     this.page.subjectName = this.subjectName 
      this.getSubjectMess(this.page)
    },
  async getSubjectMess(params) {
      var tableInfo = await list(params)
      this.tableData = tableInfo.data.items
      this.page.total = tableInfo.data.counts
     
    }
  },
  created() {
    this.getSubjectMess()
  }
}
</script>

<style scoped>
</style>
