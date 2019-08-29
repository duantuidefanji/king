<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-form>
          <!-- 学科 -->
          <el-form-item label="学科：">
            <el-select v-model="formData.subjectNameId"  placeholder="请选择">
              <el-option
              v-for="item in getManyData.subjectName"
              :key="item.value"
              :value="item.value"
              :label="item.label"></el-option>
            </el-select>
          </el-form-item>

          <!-- 目录 -->
          <el-form-item label="目录：">
            <el-selct v-model="formData.simpleDirectoryId">
              <el-option></el-option>
            </el-selct>
          </el-form-item>
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
// import { simple as simpleDirectory } from '@/api/hmmm/directorys'
// 企业管理列表
// import { list } from '@/api/hmmm/companys'
// 城市、地区
// import { datas } from '@/api/hmmm/citys'
export default {
  name: 'QuestionsNew',
  data() {
    return {
      formData: {
        subjectNameId: '' // 学科Id
      },
      checkList: [],
      getManyData: {
        subjectName: '' // 学科名称
      }
    }
  },
  methods: {
    changeQuestionType() {
      console.log(arguments)
    },

    getData() {
      // 学科简单列表
      simple().then(result => {
        // console.log(result)
        this.getManyData.subjectName = result.data
        console.log(this.getManyData.subjectName)
      })
    }
  },
  async created() {
    this.getData()
  }
}
</script>

<style scoped>
</style>
