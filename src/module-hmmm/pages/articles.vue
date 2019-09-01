<template>
  <el-card class="view-el-card">
    <div slot="header">
      <i class="el-icon-s-fold" style="font-size:25px"></i>
      <el-button
        style="float: right; padding: 3px 0; font-size:25px; color:black; margin-left:20px"
        type="text"
        class="el-icon-bell"
      ></el-button>
      <el-button
        style="float: right; padding: 3px 0; font-size:25px; color:black"
        type="text"
        class="el-icon-search"
      ></el-button>
    </div>
    <el-row>
      <el-col :span="24">
        <el-button
          class="view-btn"
          style="font-size:12px; padding:8px 12px;back-ground:f3f3f3"
          @click="viewGo"
        >新增面试技巧</el-button>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="12" class="view-el-col">
        <div class="vuew-key">
          <label for="view-ipt">关键字</label>
          <input type="text" id="view-ipt" placeholder="请输入题目编号或题干" />
        </div>
      </el-col>
      <el-col :span="12" class="view-el-col">
        <el-button string="mini" type="primary" class="view-public">
          <span>搜 索</span>
        </el-button>
        <el-button string="mini" class="view-public">
          <span>清 除</span>
        </el-button>
      </el-col>
    </el-row>
    <el-table :data="formData" border style="width: 100%; font-size:12px">
      <el-table-column prop="id" label="序号" width="94" height="50"></el-table-column>
      <el-table-column prop="title" label="标题" width="392" height="50"></el-table-column>
      <el-table-column prop="reads" label="阅读数" width="114" height="50"></el-table-column>
      <el-table-column prop="state" label="状态" width="89" height="50"></el-table-column>
      <el-table-column prop="creator" label="录入人" width="126" height="50"></el-table-column>
      <el-table-column prop="operate" label="操作" width="216" height="50">
        <template slot-scope="stData">
          <a href="#">预览</a>
          <el-button type="text" @click="getItem(stData.row)">修改</el-button>
          <a href="javascript:;" @click="delItem(stData.row)">删除</a>
          <a href="#">禁用</a>
        </template>
      </el-table-column>
    </el-table>
    <el-row type="flex" justify="center" style="margin:20px 0">
      <el-pagination
        :current-page.sync="page.currentPage"
        :page-size="page.pagesize"
        layout="prev, pager, next, jumper"
        :total="page.total"
      ></el-pagination>
    </el-row>
    <el-dialog title="编辑内容" :visible.sync="dialogVisible" width="20%">
      <div style="text-align:right">
        <p>
          <label for="view-title">标题:</label>
          <input type="text" id="view-title" v-model="edit.title" />
        </p>
        <p>
          <label for="view-reads">文章正文:</label>
          <textarea v-model="edit.aritcleBody" cols="30" rows="10"></textarea>
        </p>

        <label for="view-state">视频链接:</label>
        <input type="text" id="view-state" v-model="edit.videoURL" />
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click=" editItem() ">确 定</el-button>
      </span>
    </el-dialog>
  </el-card>
</template>

<script>
// import Vue from 'vue'
import { list, remove, detail, update } from '@/api/hmmm/articles'

export default {
  name: 'ArticlesList',
  data() {
    return {
      formData: [],
      page: {
        pagesize: 10,
        currentPage: 1,
        total: 0
      },
      dialogVisible: false,
      edit: {
        id: '',
        title: '',
        reads: '',
        states: '',
        creator: '',
        aritcleBody: '',
        videoURL: ''
      }
    }
  },
  methods: {
    viewGo() {
      this.$router.push('/articles/add')
    },
    getItem(id) {
      this.dialogVisible = true
      detail(id).then(result => {
        this.edit = result.data
      })
    },
    editItem(id) {
      this.dialogVisible = false
      update(this.edit).then(result => {
        this.lists()
      })
    },
    delItem(id) {
      // console.log(id)
      remove(id).then(() => {
        // console.log(info)
        this.lists()
      })
    },
    lists() {
      list().then(result => {
        this.formData = result.data.items
        this.page.currentPage = result.data.page
        this.page.total = result.data.pages
        this.page.pagesize = result.data.pagesize
      })
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`)
    }
  },
  created() {
    this.lists()
  }
}
</script>

<style scoped lang="less">
.vuew-key {
  width: 200px;
  height: 28px;
  background-color: #f2f2f2;
  font-size: 12px;
  line-height: 28px;
  #view-ipt {
    width: 152px;
    height: 28px;
    padding: 10px;
    float: right;
    border-radius: 5px;
    border: none;
    border: 1px solid #ccc;
    outline: medium;
  }
}
.view-public {
  font-size: 12px;
  float: right;
  margin-left: 10px;
}
.view-el-col {
  margin-top: 10px;
}
</style>
