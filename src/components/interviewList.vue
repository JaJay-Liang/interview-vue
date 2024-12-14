<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

<!--  关键字输入  -->
<!--   筛选页 -->
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item>
        <el-input
            placeholder="请输入内容"
            v-model="formInline.searchKey"
            clearable>
        </el-input>
      </el-form-item>


      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>

      <el-form-item>
        <el-input
            placeholder="默认返回10个结果"
            v-model="formInline.pagesize"
            clearable>
        </el-input>
      </el-form-item>
    </el-form>


    <el-table
        :data="interviewTest"
        border
        style="width: 100%">
      <el-table-column
          prop="id"
          label="题号"
          width="180">
      </el-table-column>

      <el-table-column
          prop="title"
          label="题目"
          width="360">
      </el-table-column>
      <el-table-column
          prop="answer"
          label="答案"
          width="360">
      </el-table-column>
      <el-table-column
          label="答案隐藏版"
          width="360">
        <template slot-scope="scope">
          <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
          <div id="answer" style="display: none">
            {{scope.row.answer}}
          </div>
        </template>
      </el-table-column>
      <el-table-column
          prop="k_type"
          label="类型"
          width="180">
      </el-table-column>
      <el-table-column
          label="请填写">
        <textarea cols="100" rows="4"></textarea>
      </el-table-column>
    </el-table>

  </div>

</template>



<script>

import axios from 'axios'

export default {
  name: "interviewList.vue",
  props: {
    msg: String
  },
  data() {
    return {
      point: '',
      formInline: {
        searchKey: '',
        pagesize: 10
      },
      interviewTest: [],
      loading: true
    }
  },
  methods: {
    onSubmit() {
      const that = this;
      //发送ajax请求，获取返回结果
      axios({
        timeout: 700,
        method:"post",
        url:"http://localhost:58021/interview/api/interview/list",
        data: this.formInline
      }).then(function (resp){
        console.log(resp.data.data)
        that.interviewTest = resp.data.data
      }).catch(() => {
        this.$message({
          showClose: true,
          message: '请求失败',
          type: 'error'
        });
      });
    },
    handleClick(row){
      console.log(row)
      this.$confirm(row.answer, '提示', {
        confirmButtonText: '确定',
      }).then(() => {
      }).catch(() => {
      });
    }

  }

}
</script>

<style>
body {
  margin: 0;
}
</style>

<style scoped>

</style>