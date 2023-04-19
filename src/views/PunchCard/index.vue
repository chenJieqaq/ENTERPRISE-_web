<template>
  <div>
    <el-form :model="form">
      <el-form-item label="考勤城市">
        <el-input v-model="form.city" />
      </el-form-item>
      <el-form-item label="考勤时间">
        <el-date-picker v-model="form.date" type="datetime" :editable="false" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="checkIn">打卡</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      form: {
        city: '',
        date: new Date()
      }
    }
  },

  methods: {
    checkIn() {
      // 获取当前位置信息
      navigator.geolocation.getCurrentPosition((position) => {
        const data = {
          lat: position.coords.latitude,
          lng: position.coords.longitude
        }
        console.log(position.coords.latitude)
        console.log(position.coords.longitude)
        // 发送位置信息到后端接口
        // axios.post('localhost:8077/api/location',
        //   {
        //   lat: position.coords.latitude,
        //   lng: position.coords.longitude,
        // }).then((response) => {
        //   // 将获取到的城市信息填充到表单中
        //   console.log(response.data)
        //   this.form.city = response.data.city;
        //   // 弹出提示框
        //   this.$message({
        //     message: '打卡成功',
        //     type: 'success',
        //   });
        // }).catch((error) => {
        //   // 打卡失败，弹出错误提示框
        //   this.$message({
        //     message: '打卡失败：' + error.response.data.message,
        //     type: 'error',
        //   });
        // });
        // 发送 POST 请求到后端接口

        axios.post('http://localhost:8077/api/location', data, {
          headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer eyJhbGciOiJIUzUxMiJ9.eyJqdGkiOiJjMDIzNzAwOTIxZTU0MjI0ODk3YmZmMzliZjY1NjIxYyIsInVzZXIiOiJhZG1pbiIsInN1YiI6ImFkbWluIn0.ALNJdthOb8SQNbDyImlzK4oK17LqGxDJzb4he1DN6FdKrnHaaGgWNH6audToUWMGTu3-bVMxWYgfjIOUf-Z_oA'
          }
        }).then(response => {
          // console.log(response)
          this.form.city = response.data

          // 弹出提示框
          this.$message({
            message: '打卡成功',
            type: 'success'
          })
          // 处理响应数据
        }).catch(error => {
          console.log(error)
          // 处理错误
        })
      })
    }
  }
}
</script>
