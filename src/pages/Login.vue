<template>
  <div class="container">
    <!-- 关闭的按钮 -->
    <div class="close">
      <span class="iconfont iconicon-test"></span>
    </div>

    <!-- logo -->
    <div class="logo">
      <span class="iconfont iconnew"></span>
    </div>

    <!-- 用户名、昵称、密码输入框 -->
    <div class="inputs">
      <div>
        <AuthInput
          placeholder="电话号码"
          :value="form.username"
          @input="handleUsername"
          :rule="/^1[0-9]{4,10}$/"
          err_message="电话号码格式不正确"
        ></AuthInput>
      </div>
      <div>
        <AuthInput
          placeholder="密码"
          v-model="form.password"
          :rule="/^[0-9a-zA-Z]{3,12}$/"
          err_message="密码格式不正确"
        ></AuthInput>
      </div>
    </div>
    <div class="goRegister">
      没有账户？
      <span>
        <router-link to="/register">去注册</router-link>
      </span>
    </div>
    <AuthButton text="登录" @click="handleSubmit" class="login"></AuthButton>
  </div>
</template>

<script>
//导入组件
import AuthInput from "@/components/AuthInput";
import AuthButton from "@/components/AuthButton";

export default {
  data() {
    return {
      form: {
        username: "",
        password: "",
        nickname: ""
      }
    };
  },
  components: {
    AuthInput,
    AuthButton
  },
  methods: {
    handleUsername(value) {
      this.form.username = value;
    },
    handleSubmit() {
      this.$axios({
        url: "/login",
        method: "POST", // method相当于type
        data: this.form
        // .then的回调函数相当于success
      }).then(res => {
        const { message, data } = res.data;
        if (message === "登录成功") {
          this.$toast.success(message);
          localStorage.setItem("token", data.token);
          localStorage.setItem("user_id", data.user.id);
          this.$router.push("/personal");
        }
      });
    }
  }
};
</script>

<style scoped lang="less">
// lang声明样式的类型
.container {
  padding: 20px;
  .login {
    font-size: 18px;
  }
  .goRegister {
    text-align-last: right;
    margin-bottom: 20px;
    font-size: 14px;
    span {
      color: #2319dc;
    }
  }
  .close {
    span {
      font-size: 27 / 360 * 100vw;
    }
  }
  .logo {
    display: flex;
    justify-content: center;
    span {
      display: block;
      font-size: 126 / 360 * 100vw;
      color: #d81e06;
    }
  }
  .inputs {
    .input {
      margin-bottom: 20px;
    }
  }
}
</style>

