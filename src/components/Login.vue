<template>
  <div class="login_contanier">
    <div class="login_box">
      <div class="avatar_box">
        <img src="../assets/logo.png" />
      </div>
      <div>
        <el-form
          :model="loginForm"
          ref="loginFormRef"
          :rules="loginFormRules"
          label-width="0px"
          class="login_form"
        >
          <el-form-item prop="username">
            <el-input v-model="loginForm.username" prefix-icon="el-icon-view"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input v-model="loginForm.password" prefix-icon="el-icon-sold-out" type="password"></el-input>
          </el-form-item>
          <el-form-item class="btns">
            <el-button type="primary" @click="login">登录</el-button>
            <el-button type="info" @click="resetLoginForm">取消</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      loginForm: {
        username: "zs",
        password: "123456"
      },
      //   表单验证
      loginFormRules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 18, message: "密码为6—18位", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    //   取消按钮
    resetLoginForm() {
      //   console.log(this);
      this.$refs.loginFormRef.resetFields();
    },
    // 登录
    login() {
      // 表单预验证
      this.$refs.loginFormRef.validate(async valid => {
        // console.log(valid);
        if (!valid) return;
        const { data: res } = await this.$http.post("login", this.loginForm);
        // console.log(res);
        if (res.meta.status !== 200) return this.$message.error("登录失败");
        this.$message.success("登录成功");
        // 登录成功后跳转主页
        window.sessionStorage.setItem("token",res.data.token);
        this.$router.push('/home');
      });
    }
  }
};
</script>

<style lang="less" scoped>
.login_contanier {
  background: #2b4b6b;
  height: 100%;
}
.login_box {
  background: #fff;
  width: 450px;
  height: 300px;
  border-radius: 3px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  .avatar_box {
    width: 130px;
    height: 130px;
    border: 1px solid #eee;
    padding: 10px;
    border-radius: 50%;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;

    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background: #eee;
    }
  }
}
.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btns {
  display: flex;
  justify-content: flex-end;
}
</style>
