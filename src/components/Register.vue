<template>
  <div class="login clearfix">
    <div class="login-wrap">
      <el-row type="flex" justify="center">
        <el-form ref="loginForm" :model="user" status-icon label-width="80px">
          <h3>注册</h3>
          <hr>
          <el-form-item prop="username" label="用户名">
            <el-input v-model="user.username" placeholder="请输入用户名"></el-input>
          </el-form-item>
          <el-form-item prop="password" label="设置密码">
            <el-input v-model="user.password" show-password placeholder="请输入密码"></el-input>
          </el-form-item>
          <el-form-item prop="repassword" label="确认密码">
            <el-input v-model="user.repassword" show-password placeholder="请再次输入密码"></el-input>
          </el-form-item>
          <el-form-item prop="gender" label="性别">
            <el-radio v-model="user.gender" label="男" @change="getValue()">男</el-radio>
            <el-radio v-model="user.gender" label="女" @change="getValue()">女</el-radio>
          </el-form-item>
          <el-form-item prop="birthday" label="生日">
            <el-date-picker v-model="user.birthday" type="date" placeholder="选择日期" :picker-options="pickerOptions0">
            </el-date-picker>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" icon @click="doRegister()">注册账号</el-button>
          </el-form-item>
        </el-form>
      </el-row>
    </div>
  </div>
</template>
   
<script>
import axios from "axios";
export default {
  name: "login",
  data() {
    return {
      user: {
        username: "",
        password: "",
        repassword:"",
        gender: "",
        birthday: ""
      },
    };
  },
  created() {
    // console.log($);
    // console.log("1111");
  },
  methods: {
    doRegister() {
      if (!this.user.username) {
        this.$message.error("请输入用户名！");
        return;
      }
      else if (!this.user.password) {
        this.$message.error("请输入密码！");
        return;
      }else if (!this.user.repassword) {
        this.$message.error("请确认密码！");
        return;
      }else if (this.user.password!=this.user.repassword) {
        this.$message.error("两次输入密码不一致");
        return;
      }
       else {
        // this.$router.push({ path: "/" }); //无需向后台提交数据，方便前台调试
        axios
          .post("http://localhost/user/register", {
            username: this.user.username,
            password: this.user.password,
            gender: this.user.gender,
            birthday: this.user.birthday
          })
          .then(res => {
            if (res.data.success) {
              alert("注册成功~");
              this.$router.push({ path: "/" });
            } else {
              alert("您输入的用户名已存在~");
            }
          });
      }
    }
  }
};
</script>
   
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.login {
  width: 100%;
  height: 740px;

  background-size: cover;
  overflow: hidden;
}

.login-wrap {

  background-size: cover;
  width: 400px;
  height: 400;
  margin: 215px auto;
  overflow: hidden;
  padding-top: 10px;
  line-height: 20px;
}

h3 {
  color: #0babeab8;
  font-size: 24px;
}

hr {
  background-color: #444;
  margin: 20px auto;
}

.el-button {
  width: 80%;
  margin-left: -50px;
}
</style>