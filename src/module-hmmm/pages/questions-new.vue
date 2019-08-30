<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-form>
          <!-- 学科 -->
          <el-form-item label="学科：">
            <el-select v-model="formData.subjectNameId" placeholder="请选择">
              <el-option
                v-for="item in subjectName"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-form-item>

          <!-- 目录 -->
          <el-form-item label="目录：">
            <el-select v-model="formData.simpleDirectoryId">
              <el-option
                v-for="item in directionName"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-form-item>

          <!-- 企业： -->
          <el-form-item label="企业：">
            <el-select v-model="formData.shortName">
              <el-option
                v-for="item in shortName"
                :key="item.id"
                :value="item.id"
                :label="item.shortName"
              ></el-option>
            </el-select>
          </el-form-item>

          <!-- 城市 -->
          <el-form-item label="城市：">
            <el-select v-model="formData.provinces">
              <el-option
                v-for="(item,index) in provinces"
                :key="index"
                :value="index"
                :label="item"
              ></el-option>
            </el-select>
          </el-form-item>

          <!-- 方向 -->
          <el-form-item label="方向：">
            <el-select v-model="formData.direction">
              <el-option
                v-for="(item,index) in direction"
                :key="index"
                :value="index"
                :label="item"
              ></el-option>
            </el-select>
          </el-form-item>

          <!-- 题型 -->
          <el-form-item label="题型：">
            <el-radio-group @change="changeQuestionType" v-model="formData.questionType">
              <el-radio
                v-for="item in questionType"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-radio>
            </el-radio-group>
          </el-form-item>

          <!-- 难度 -->
          <el-form-item label="难度：">
            <el-radio-group v-model="formData.difficulty">
              <el-radio
                v-for="item in difficulty"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-radio>
            </el-radio-group>
          </el-form-item>

          <!-- 题干： -->
          <el-form-item label="题干：">
            <el-input></el-input>
          </el-form-item>

          <!-- 选项 -->
          <el-form-item v-if="chooseBoolean" label="选项（以下选中的选项为正确答案）">
            <br />
            <el-radio-group v-if="chooseRadio" v-model="formData.option">
              <el-radio style="margin-top:15px" :value="option.isRight">
                A：
                <el-input v-model="formData.option.title"></el-input>
              </el-radio>
              <br />
              <el-radio style="margin-top:15px" :value="option.isRight">
                B：
                <el-input v-model="formData.option.title"></el-input>
              </el-radio>
              <br />
              <el-radio style="margin-top:15px" :value="option.isRight">
                C：
                <el-input v-model="formData.option.title"></el-input>
              </el-radio>
              <br />
              <el-radio style="margin-top:15px" :value="option.isRight">
                D：
                <el-input v-model="formData.option.title"></el-input>
              </el-radio>
              <br />
            </el-radio-group>

            <!-- 复选框 -->
            <el-checkbox-group v-if="chooseCheckbox" v-model="formData.option">
              <el-checkbox style="margin-top:15px" :value="option.isRight">
                A：
                <el-input v-model="formData.option.title"></el-input>
              </el-checkbox>
              <br />
              <el-checkbox style="margin-top:15px" :value="option.isRight">
                B：
                <el-input v-model="formData.option.title"></el-input>
              </el-checkbox>
              <br />
              <el-checkbox style="margin-top:15px" :value="option.isRight">
                C：
                <el-input v-model="formData.option.title"></el-input>
              </el-checkbox>
              <br />
              <el-checkbox style="margin-top:15px" :value="option.isRight">
                D：
                <el-input v-model="formData.option.title"></el-input>
              </el-checkbox>
              <br />
            </el-checkbox-group>
          </el-form-item>

          <!-- 答案解析 -->
          <el-form-item label="答案解析">
            <el-input></el-input>
          </el-form-item>

          <!-- 题目备注 -->
          <el-form-item label="题目备注:">
            <el-input></el-input>
          </el-form-item>

          <!-- 试题标签 -->
          <el-form-item label="试题标签：">
            <el-input></el-input>
          </el-form-item>

          <el-row type="flex" justify="center">
            <el-form-item>
              <el-button type="primary">提交</el-button>
              <el-button>取消</el-button>
            </el-form-item>
          </el-row>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
// 题型：questionType, 难度:difficuity, 方向：direction
import { questionType, difficulty, direction } from '@/api/hmmm/constants'
// 学科简单列表:simple,
import { simple } from '@/api/hmmm/subjects'
// 目录简单列表:simpleDirectory
import { simple as simpleDirectory } from '@/api/hmmm/directorys'
// 企业管理列表
import { list } from '@/api/hmmm/companys'
// 城市、地区
import { provinces } from '@/api/hmmm/citys'

export default {
  name: 'QuestionsNew',
  data() {
    return {
      formData: {
        subjectNameId: '', // 学科Id
        simpleDirectoryId: '', // 目录Id
        shortName: '', // 公司简称
        provinces: '', // 省份
        direction: '', // 方向
        questionType: '', // 题型
        difficulty: '', // 难度
        option: {
          code: '', // 代码
          title: '', // 标题
          img: '', // 图片URL
          isRight: false // 是否正确答案
        }
      },
      checkList: [],
      subjectName: '', // 学科名称
      directionName: '', // 目录名称
      shortName: [], // 企业简称
      provinces: [], // 省份
      direction, // 方向
      questionType, // 题型
      difficulty, // 难度
      option: {
        code: '', // 代码
        title: '', // 标题
        img: '', // 图片URL
        isRight: false // 是否正确答案
      },
      chooseRadio: false, // 选项中的单选
      chooseCheckbox: false, // 选项中的复选框
      chooseBoolean: true // 选项是否消失
    }
  },
  methods: {
    changeQuestionType(params) {
      console.log(params)
      if (params === '单选') {
        // 点击单选时将选项中的复选框删除
        this.chooseCheckbox = false
        this.chooseBoolean = true
        this.chooseRadio = true
      } else if (params === '多选') {
        // 点击复选时将单选项组删除
        this.chooseRadio = false // 将单选设为false
        this.chooseBoolean = true
        this.chooseCheckbox = true
      } else {
        // 点击简答时，将单选项组删除、复选项组删除
        this.chooseRadio = false
        this.chooseCheckbox = false
        this.chooseBoolean = false
      }
    },

    getData() {
      // 学科简单列表
      simple().then(result => {
        // console.log(result)
        this.subjectName = result.data
      })

      // 目录简单列表
      simpleDirectory().then(result => {
        this.directionName = result.data
      })

      // 企业名称列表
      list().then(result => {
        this.shortName = result.data.items
      })

      // 省份
      this.provinces = provinces()
      console.log(this.provinces)
    }
  },
  async created() {
    this.getData()
    this.chooseRadio = true
  }
}
</script>

<style scoped>
</style>
