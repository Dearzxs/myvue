<template>
  <div class="back-login">
    <el-form :model="form" :rules="rules" ref="loginForm" label-width="100px" class="login-box">
      <h3 class="login-title">欢迎使用教学视频语音提取文本系统</h3>
      <el-form-item label="账号" prop="username">
        <el-input v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="form.password"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm()">登录</el-button>
      </el-form-item>
    </el-form>
    <el-dialog title="温馨提示" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <span>请输入账号和密码</span>
      <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="dialogVisible = false">确定</el-button>
        </span>
    </el-dialog>
  </div>
</template>

<script>
import Qs from 'qs'

export default {
  data() {
    return {
      form: {
        username: '',
        password: '',
      },
      rules: {
        username: [
          {required: true, message: '请输入用户名', trigger: 'blur'},
          {min: 5, max: 10, message: '长度在 5 到 10 个字符', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          {min: 5, max: 10, message: '长度在 5 到 10 个字符', trigger: 'blur'}
        ]
      },
      dialogVisible: false
    };
  },
  methods: {
    submitForm() {
      let param = Qs.stringify({
        "userId":this.form.username,
        "userPwd":this.form.password
      });
      this.$axios.post('login/judge',param).then(res => {
        if (res.data === 'success') {
          sessionStorage.setItem('userId', this.form.username);
          this.$router.push('/')
          alert('登陆成功')
        } else {
          alert('登录失败，请检查用户名和密码')
        }
      })
    },
    handleClose: function () {
      console.log("Handle Close，空函数");
    },
  }

}
</script>

<style scoped>

.back-login {
  background-image: linear-gradient(to right, #16222A, #3A6073);
  width: 100%;
  height: 100%;
  position: fixed;
  background-size: 100% 100%;
}

.login-box {
  border: 1px solid #DCDFE6;
  background-color: #FFFFFF;
  width: 350px;
  margin: 180px auto;
  padding: 20px 35px;
  border-radius: 5px;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  box-shadow: 0 0 25px #909399;
}

.login-title {
  text-align: center;
  margin: 0 auto 40px auto;
  color: #303133;
}
</style>
