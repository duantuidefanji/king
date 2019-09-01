<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-form>
          <el-form-item>
            <el-button>新增试题</el-button>
            <el-button>批量导入</el-button>
          </el-form-item>

          <el-row type="flex" justify="space-around">
            <el-form-item label-width="55px" label="学科:" style="width:160px">
              <el-select v-model="formData.subjectID" placeholder="请选择">
                <el-option
                  v-for="item in subject"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label-width="60px" label="难度:" style="width:160px">
              <el-select v-model="formData.difficultyID">
                <el-option
                  v-for="item in difficulty"
                  :key="item.value"
                  :value="item.value"
                  :label="item.label"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label-width="80px" label="试题类型:" style="width:180px">
              <el-select v-model="formData.questionTypeID">
                <el-option
                  v-for="item in questionType"
                  :key="item.value"
                  :value="item.value"
                  :label="item.label"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label-width="60px" label="标签:" style="width:160px">
              <el-select v-model="formData.tagsID">
                <el-option
                  v-for="item in tags"
                  :key="item.id"
                  :value="item.id"
                  :label="item.tagName"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label-width="60px" label="城市" style="width:360px">
              <el-select
                style="width:100px;margin-right:10px"
                v-model="formData.provincesID"
                @change="provincesChange"
              >
                <el-option
                  v-for="(item,index) in provinces"
                  :key="index"
                  :value="item"
                  :label="item"
                ></el-option>
              </el-select>

              <el-select style="width:100px" v-model="formData.areaID">
                <el-option v-for="(item,index) in area" :key="index" :value="index" :label="item"></el-option>
              </el-select>
            </el-form-item>
          </el-row>

          <el-row type="flex">
            <el-form-item label-width="60px" label="关键字" style="width:250px">
              <el-input placeholder="请输入题目编号/题干"></el-input>
            </el-form-item>

            <el-form-item label-width="80px" label="题目备注" style="width:250px">
              <el-input placeholder="请输入题目备注"></el-input>
            </el-form-item>

            <el-form-item label-width="80px" label="企业简称" style="width:250px">
              <el-input placeholder="请输入企业简称"></el-input>
            </el-form-item>

            <el-form-item label-width="60px" label="方向" style="width:180px">
              <el-select v-model="formData.directionID">
                <el-option
                  v-for="(item,index) in direction"
                  :key="index"
                  :value="index"
                  :label="item"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-row>

          <el-row type="flex">
            <el-form-item label-width="75px" label="二级目录" style="width:220px">
              <el-input placeholder="请输入二级目录"></el-input>
            </el-form-item>
            <el-form-item style="margin-left:20px">
              <el-button>清除</el-button>
              <el-button type="primary">搜索</el-button>
            </el-form-item>
          </el-row>
        </el-form>

        <el-table :data="dataList">
          <el-table-column align="center" prop="id" label="序号"></el-table-column>
          <el-table-column align="center" prop="number" label="试题编号"></el-table-column>
          <el-table-column align="center" prop="catalogID" label="学科"></el-table-column>
          <el-table-column align="center" prop="questionType" label="题型"></el-table-column>
          <el-table-column align="center" prop="question" label="题干"></el-table-column>
          <el-table-column align="center" prop="addDate" label="录入时间"></el-table-column>
          <el-table-column align="center" prop="difficulty" label="难度"></el-table-column>
          <el-table-column align="center" label="使用次数"></el-table-column>
          <el-table-column align="center" prop="creator" label="录入人"></el-table-column>
          <el-table-column align="center" label="操作">
            <template slot-scope="scope">
              <el-button @click="edit(scope.row.id)">修改</el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-card>
    </div>
  </div>
</template>

<script>
// 学科列表
import { simple } from '@/api/hmmm/subjects.js'
//   难度:difficulty,题型：questionType,方向：direction
import { difficulty, questionType, direction } from '@/api/hmmm/constants.js'
// 标签列表
import { list } from '@/api/hmmm/tags.js'
// 城市、地区
import { provinces, citys } from '@/api/hmmm/citys.js'
// 基础题库列表:baseList , 更新题库update
import { list as baseList, update } from '@/api/hmmm/questions.js'
export default {
  name: 'QuestionsList',
  data() {
    return {
      formData: {
        subjectID: '',
        difficultyID: '',
        questionTypeID: '',
        tagsID: '',
        provincesID: '',
        areaID: '',
        directionID: ''
      },
      subject: '',
      difficulty,
      questionType,
      tags: '',
      provinces: '',
      area: '',
      direction,
      dataList: '',
      dialogVislible: false
    }
  },
  methods: {
    // 初始化数据
    async initialData() {
      // 学科
      const { data: subject } = await simple()
      this.subject = subject

      // 标签
      const { data: tags } = await list()
      this.tags = tags

      // 城市
      let provincesResult = provinces()
      this.provinces = provincesResult

      // 基础题库列表
      const { data: res } = await baseList()
      this.dataList = res.items
      console.log(this.dataList)
    },

    // 城市变换时的函数
    provincesChange(params) {
      this.area = citys(params)
    },

    // 修改函数
    edit(id) {
      this.$router.push(`/questions/new/${id}`)
    }
  },
  created() {
    this.initialData()
  }
}
</script>

<style scoped>
</style>
