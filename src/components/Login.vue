<template>
  <div class="login">
    <div class="login-wrap">
      <el-row type="flex" justify="center">
        <el-form ref="loginForm" :model="user" :rules="rules" status-icon label-width="80px">
          <h3>登录</h3>
          <hr>
          <el-form-item prop="username" label="用户名">
            <el-input v-model="user.username" placeholder="请输入用户名" prefix-icon></el-input>
          </el-form-item>
          <el-form-item id="password" prop="password" label="密码">
            <el-input v-model="user.password" show-password placeholder="请输入密码"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" icon="el-icon-user" @click.native="goUrl('/register')">注册账号</el-button>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" icon="el-icon-upload" @click="doLogin()">登 录</el-button>
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
        password: ""
      }
    };
  },
  created() { },
  methods: {
    goUrl(url) {
      window.location.href = url
    },
    doLogin() {
      if (!this.user.username) {
        this.$message.error("请输入用户名！");
        return;
      } else if (!this.user.password) {
        this.$message.error("请输入密码！");
        return;
      } else {
        axios
          .post("http://localhost/user/login", {
            username: this.user.username,
            password: this.user.password
          })
          .then(res => {
            if (res.data.success) {
              alert("登录成功~");
            } else {
              alert("您输入的用户名或密码错误~~");
            }
          });
      }
    }
  }
};
</script>
 

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