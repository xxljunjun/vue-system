<template>
  <div class="changePassword">
    <el-form
      ref="resetPasswordForm"
      :model="loginForm"
      :rules="loginRules"
      label-position="left"
      class="resetPassword"
      label-width="100px"
    >
      <el-form-item prop="username" label="用户名：">
        <el-input
          ref="username"
          v-model="loginForm.username"
          placeholder="用户名"
          name="username"
          type="text"
          tabindex="1"
        />
      </el-form-item>

      <el-form-item prop="password" label="新密码：">
        <el-input
          ref="password"
          v-model="loginForm.password"
          type="text"
          placeholder="新密码"
          name="password"
          tabindex="2"
        />
      </el-form-item>

      <el-form-item prop="checkPassword" label="确认密码：">
        <el-input
          ref="checkPassword"
          v-model="loginForm.checkPassword"
          type="text"
          placeholder="确认密码"
          name="checkPassword"
          tabindex="2"
        />
      </el-form-item>
      <div class="sub_box">
          <el-button
        :loading="loading"
        type="primary"
        style="width: 100px; margin-bottom: 30px"
        @click.native.prevent="submit"
        >提交</el-button
      >
      </div>
     
    </el-form>
  </div>
</template>

<script>
import { validUsername } from "@/utils/validate";
export default {
  name: "changePassword",
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!validUsername(value)) {
        callback(new Error("请输入正确的用户名！"));
      } else {
        callback();
      }
    };
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error("密码长度应超过6位！"));
      } else {
        callback();
      }
    };
    const validatecheckPassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error("密码长度应超过6位！"));
      } else {
        callback();
      }
    };
    return {
      loading: false,
      loginForm: {
        username: "admin",
        password: "111111",
        checkPassword: "111111",
      },
      loginRules: {
        username: [
          { required: true, trigger: "blur", validator: validateUsername },
        ],
        password: [
          { required: true, trigger: "blur", validator: validatePassword },
        ],
        checkPassword: [
          { required: true, trigger: "blur", validator: validatecheckPassword },
        ],
      },
    };
  },
  computed: {},
  methods: {
    submit() {
      this.$refs.resetPasswordForm.validate((valid) => {
        console.log(">>Boolean值",valid)
        if (valid) {
          this.loading = true;
          setTimeout(()=>{
            this.loading = false
          },1000)
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.changePassword {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  width: 400px;
  height: 400px;
  .resetPassword {
    .el-form-item {
    }
    .sub_box{
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
}
</style>
