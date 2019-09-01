<template>
  <div class="dashboard-container">
    <el-card>
      <el-form ref="form">
        <el-form-item>
          <el-button type="info" @click=edit>新增试题</el-button>
          <el-button type="info">批量导入</el-button>
        </el-form-item>
        <el-form-item>
          <el-row :gutter="20">
            <el-col :span="6">
              <span class="subject">学科</span>
              <el-select v-model="formData.subjectID"  placeholder="请选择" style="width:150px;margin-bottom:20px;">
                <el-option
                  v-for="item in subjectID"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-col>
            <el-col :span="6">
              <span class="subject">状态</span>
              <el-select v-model="formData.status" placeholder="请选择" style="width:150px">
                <el-option
                  v-for="item in statusList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-col>
            <el-col :span="6">
              <span class="subject">难度</span>
              <el-select v-model="formData.difficulty" placeholder="请选择" style="width:150px">
                <el-option
                  v-for="item in difficultyList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-col>
            <el-col :span="6">
              <span class="subject">实体类型</span>
              <el-select v-model="formData.questionType" placeholder="请选择" style="width:150px">
                <el-option
                  v-for="item in questionTypeList"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-col>
          </el-row>
          <!-- 第二排 -->
          <el-row :gutter="20">
            <el-col :span="6">
              <span class="subject">标签</span>
              <el-input style="width:100px" placeholder="请输入" v-model="formData.tags"></el-input>
              <span class="subject">输入人</span>
              <el-input style="width:100px" placeholder="请输入"  v-model="formData.creatorID"></el-input>
            </el-col>
            <el-col :span="6">
              <span class="subject">关键字</span>
              <el-input style="width:180px;margin-bottom:20px" placeholder="请输入题目编号/题干"
               v-model="formData.keyword"></el-input>
            </el-col>
            <el-col :span="6">
              <span class="subject">题目备注</span>
              <el-input style="width:100px" placeholder="请输入" v-model="formData.remarks"></el-input>
            </el-col>
            <el-col :span="6">
              <span class="subject">二级目录</span>
              <el-input style="width:150px" placeholder="请输入二级目录" v-model="formData.catalogID"></el-input>
            </el-col>
          </el-row>

          <el-row :gutter="15">
            <el-col :span="6">
              <span class="subject">城市</span>
              <el-select v-model=" formData.provinces" placeholder="请选择" style="width:100px" @change='changeCity'>
                <el-option
                  v-for="item in provincesList"
                  :key="item"
                  :label="item"
                  :value="item"
                ></el-option>
              </el-select>
              <el-select v-model="formData.citys" placeholder="请选择" style="width:100px;">
                <el-option
                  v-for="item in citysList"
                  :key="item"
                  :label="item"
                  :value="item"
                ></el-option>
              </el-select>
            </el-col>
            <el-col :span="6">
              <span class="subject">企业简称</span>
              <el-input style="width:100px" placeholder="请输入"  v-model="formData.shortName"></el-input>
            </el-col>
            <el-col :span="6">
              <span class="subject">方向</span>
              <el-input style="width:100px" placeholder="请输入" v-model="formData.direction"></el-input>
            </el-col>
            <el-col :span="6">
              <el-button plain @click='clear'>清除</el-button>
              <el-button plain type="primary" @click="search">搜索</el-button>
            </el-col>
          </el-row>
        </el-form-item>
        <!-- 第三排 -->
        <el-form-item>
          <el-tabs v-model="activeName" type="border-card">
            <el-tab-pane label="全部" name="first">
              <el-table :data="questions" style="width:100%">
                <el-table-column prop="id" label="序号"></el-table-column>
                <el-table-column prop="number" label="试题编号"></el-table-column>
                <el-table-column prop="subjectID" label="学科" :formatter="formatterSub"></el-table-column>
                <el-table-column prop="questionType" label="题型" :formatter="formatterQtype"></el-table-column>
                <el-table-column prop="question" label="题干"></el-table-column>
                <el-table-column prop="addDate" label="录入时间">
                  <span slot-scope="stData">{{stData.row.addDate | parseTimeByString}}</span>
                </el-table-column>
                <el-table-column prop="creator" label="录入人"></el-table-column>
                <el-table-column prop="difficulty" label="难度"></el-table-column>
                <el-table-column prop="useNumber" label="使用次数"></el-table-column>
                <el-table-column prop="chkState"  :formatter="formatterChk" label="审核状态"></el-table-column>
                <el-table-column prop="chkUser" label="审核人"></el-table-column>
                <el-table-column prop="publishState" label="发布状态" :formatter="formatterPub"></el-table-column>
                <el-table-column prop="operate" label="操作">
                     <template slot-scope="stData">
                  <el-link type="primary" :underline="false">审核</el-link>
                  <el-link type="primary" :underline="false">预览</el-link>
                  <el-link type="primary" :underline="false">下架</el-link>
                  <el-link type="primary" :underline="false" @click='edit(stData.row.id)'>修改</el-link>
                  <el-link type="primary" :underline="false"  @click="del(stData.row)">删除</el-link>
                     </template>
                </el-table-column>
              </el-table>
            </el-tab-pane>
            <el-tab-pane label="待审核" name="second">
               <el-table :data="questions" style="width:100%">
                <el-table-column prop="id" label="序号"></el-table-column>
                <el-table-column prop="number" label="试题编号"></el-table-column>
                <el-table-column prop="subjectID" label="学科" :formatter="formatterSub"></el-table-column>
                <el-table-column prop="questionType" label="题型" :formatter="formatterQtype"></el-table-column>
                <el-table-column prop="question" label="题干"></el-table-column>
                <el-table-column prop="addDate" label="录入时间">
                   <span slot-scope="stData">{{stData.row.addDate | parseTimeByString}}</span>
                </el-table-column>
                <el-table-column prop="creator" label="录入人"></el-table-column>
                <el-table-column prop="difficulty" label="难度"></el-table-column>
                <el-table-column prop="useNumber" label="使用次数"></el-table-column>
                <el-table-column prop="chkState" :formatter="formatterChk" label="审核状态"></el-table-column>
                <el-table-column prop="chkUser" label="审核人"></el-table-column>
                <el-table-column prop="publishState" label="发布状态" :formatter="formatterPub"></el-table-column>
                <el-table-column prop="operate" label="操作">
                     <template slot-scope="stData">
                  <el-link type="primary" :underline="false">审核</el-link>
                  <el-link type="primary" :underline="false">预览</el-link>
                  <el-link type="primary" :underline="false">下架</el-link>
                  <el-link type="primary" :underline="false" @click='edit(stData.row.id)'>修改</el-link>
                  <el-link type="primary" :underline="false"  @click="del(stData.row)">删除</el-link>
                     </template>
                </el-table-column>
              </el-table>
            </el-tab-pane>
            <el-tab-pane label="已审核" name="third">
              <el-table :data="questions" style="width:100%">
                <el-table-column prop="id" label="序号"></el-table-column>
                <el-table-column prop="number" label="试题编号"></el-table-column>
                <el-table-column prop="subjectID" label="学科" :formatter="formatterSub"></el-table-column>
                <el-table-column prop="questionType" label="题型" :formatter="formatterQtype"></el-table-column>
                <el-table-column prop="question" label="题干"></el-table-column>
                <el-table-column prop="addDate" label="录入时间">
                   <span slot-scope="stData">{{stData.row.addDate | parseTimeByString}}</span>
                </el-table-column>
                <el-table-column prop="creator" label="录入人"></el-table-column>
                <el-table-column prop="difficulty" label="难度"></el-table-column>
                <el-table-column prop="useNumber" label="使用次数"></el-table-column>
                <el-table-column prop="chkState"  :formatter="formatterChk" label="审核状态"></el-table-column>
                <el-table-column prop="chkUser" label="审核人"></el-table-column>
                <el-table-column prop="publishState" label="发布状态" :formatter="formatterPub"></el-table-column>
                <el-table-column prop="operate" label="操作">
                  <template slot-scope="stData">
                     <el-link type="primary" :underline="false">审核</el-link>
                  <el-link type="primary" :underline="false">预览</el-link>
                  <el-link type="primary" :underline="false">下架</el-link>
                  <el-link type="primary" :underline="false" @click='edit(stData.row.id)' >修改</el-link>
                  <el-link type="primary" :underline="false" @click="del(stData.row)">删除</el-link>
                  </template>
                </el-table-column>
              </el-table>
            </el-tab-pane>
          </el-tabs>
        </el-form-item>
      </el-form>
      <el-row type="flex" justify="center">
<el-pagination
  background
  layout="prev, pager, next"
  :total="page.total"
  :page-size="page.pageSize"
  :current-page="page.currentPage" @current-change='currentChange'
  >
      </el-pagination>
      </el-row>
      
    </el-card>
  </div>
</template>

<script>
import { simple } from '@/api/hmmm/subjects'
import { publishType, difficulty, questionType } from '@/api/hmmm/constants'
import { citys, provinces } from '@/api/hmmm/citys'
import { choice, remove, update } from '@/api/hmmm/questions'
import { constants } from 'fs'
import { async } from 'q'
export default {
  name: 'QuestionsList',
  data() {
    return {
     subjectID: [
      ],
      statusList: [],
      difficultyList: [],
      questionTypeList: [],
      citysList: [],
      provincesList: [],
      questions: [{
        id: '',
        number: '',
        subjectID: '',
        questionType: '',
        question: '',
        addDate: '',
        creator: '',
        difficulty: '',
        useNumber: '',
        chkState: '',
        chkUser: '',
        publishState: '',
        operate: ''
      }],
      formData: [{
        subject: null,
        status: null,
        difficulty: null,
        questionType: null,
         tags: null,
        creatorID: null,
        keyword: null,
        remarks: null,
        catalogID: null,
        citys: null,
        provinces: null,
        shortName: null,
        direction: null
      }],
      activeName: 'first',
      page: {
        total: 10,
        pageSize: 10,
        currentPage: 1
      }
    }
  },
  methods: {
    edit(info) {
      this.$router.push(`/questions/new/${info}`)
    },
    // 删除
     del(info) {
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(async() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          })
          await remove(info)
          await this.getList()
        })
      },
    // 搜索
    async search() {
      let params = this.formData
     this.getList({...params})
    },
    // 清除
    clear() {
      this.formData = {}
    },
    // 城市
    changeCity(pm) {
      this.citysList = citys(pm)
    },
    // 分页
    currentChange(newpage) {
     this.currentPage = newpage
    },
    // 审核
    formatterChk(row, column, cellValue, index) {
        if (cellValue === 0) {
          return '待审核'
        } else if (cellValue === 1) {
          return '通过'
        } else if (cellValue === 2) {
          return '拒绝'
        }
    },
    // 发布
    formatterPub(row, column, cellValue, index) {
 if (cellValue === 0) {
          return '待发布'
        } else if (cellValue === 1) {
          return '已发布'
        } else if (cellValue === 2) {
          return '已下架'
        }
    },
    // 试题类型
    formatterQtype(row, column, cellValue, index) {
       if (cellValue === '1') {
          return '单选'
        } else if (cellValue === '2') {
          return '多选'
        } else if (cellValue === '3') {
          return '简答'
        }
    },
    // 学科
    formatterSub(row, column, cellValue, index) {
      if (cellValue === 1) {
         return 'java'
      } else if (cellValue === 2) {
        return 'ios'
      } else if (cellValue === 3) {
        return '安卓'
      } else if (cellValue === 4) {
        return '前端'
      } else if (cellValue === 5) {
        return '设计'
      } else if (cellValue === 6) {
        return '产品'
      } else if (cellValue === 7) {
        return 'C++'
      } else if (cellValue === 8) {
        return '数据库'
      } else if (cellValue === 9) {
        return '算法'
      } else if (cellValue === 10) {
        return '运维'
      } else if (cellValue === 11) {
        return 'PHP'
      } else if (cellValue === 12) {
        return 'c#'
      } else if (cellValue === 13) {
        return 'C'
      } else if (cellValue === 14) {
        return 'Python'
      } else if (cellValue === 15) {
        return '大数据'
      }
    },
    // 获取
   async getList(params) {
    let question = await choice(params)
   this.questions = question.data.items
    }
  },
 async created() {
   let subject = await simple()
   this.subjectID = subject.data
   this.statusList = publishType
   this.difficultyList = difficulty
   this.questionTypeList = questionType
   this.provincesList = provinces()
   this.getList()
  }
}
</script>

<style scoped lang='less'>
.subject {
  padding: 0 10px;
  background-color: #ccc;
  text-align: center;
}
</style>
