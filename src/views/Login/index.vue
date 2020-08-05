<template>
  <div id="login">
    <div class="login_wrap">
      <ul class="mneu_tab">
        <li
          :class="{'current': item.current}"
          v-for="item in mneuTab"
          :key="item.id"
          @click="toggleMneu(item)"
        >{{item.txt}}</li>
      </ul>
      <!-- 表单 -->
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        class="login_form"
        size="medium"
      >
        <el-form-item prop="username" class="item-form">
          <label>邮箱</label>
          <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item prop="password" class="item-form">
          <label>密码</label>
          <el-input
            type="password"
            v-model="ruleForm.password"
            autocomplete="off"
            minlength="6"
            maxlength="20"
          ></el-input>
        </el-form-item>

        <el-form-item prop="passwords" class="item-form" v-show="model === 'register'">
          <label>重复密码</label>
          <el-input
            type="password"
            v-model="ruleForm.passwords"
            autocomplete="off"
            minlength="6"
            maxlength="20"
          ></el-input>
        </el-form-item>

        <el-form-item prop="code" class="item-form">
          <label>验证码</label>
          <el-row :gutter="10">
            <el-col :span="15">
              <el-input v-model.number="ruleForm.code" minlength="6" maxlength="6"></el-input>
            </el-col>
            <el-col :span="9">
              <el-button type="success" class="block">获取验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')" class="login-btn block">提交</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
import {
  stripscript,
  validateEmail,
  validatePassword,
  validateCode
} from "@/utils/validate";
export default {
  name: "login",
  data() {
    var validateusername = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入用户名"));
      } else if (validateEmail(value)) {
        callback(new Error("用户名格式有误"));
      } else {
        callback();
      }
    };
    var validatepassword = (rule, value, callback) => {
      this.ruleForm.password = stripscript(value);
      value = this.ruleForm.password;
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (validatePassword(value)) {
        callback(new Error("密码为6-20数字+字母"));
      } else {
        callback();
      }
    };
    var validatepasswords = (rule, value, callback) => {
      if(this.model === "login") {
        callback();
      }
      this.ruleForm.passwords = stripscript(value);
      value = this.ruleForm.passwords;
      if (value === "") {
        callback(new Error("请再次输入密码"));
      } else if (value != this.ruleForm.password) {
        callback(new Error("两次密码不一致"));
      } else {
        callback();
      }
    };
    var validatecode = (rule, value, callback) => {
      this.ruleForm.code = stripscript(value);
      value = this.ruleForm.code;
      if (!value) {
        return callback(new Error("请输入验证码"));
      } else if (validateCode(value)) {
        callback(new Error("验证码格式有误"));
      } else {
        callback();
      }
    };
    return {
      mneuTab: [
        { txt: "登陆", current: true, type: 'login' },
        { txt: "注册", current: false, type: 'register' }
      ],
      model: "login",
      // 表单的数据
      ruleForm: {
        username: "",
        password: "",
        passwords: "",
        code: ""
      },
      rules: {
        username: [{ validator: validateusername, trigger: "blur" }],
        password: [{ validator: validatepassword, trigger: "blur" }],
        passwords: [{ validator: validatepasswords, trigger: "blur" }],
        code: [{ validator: validatecode, trigger: "blur" }]
      }
    };
  },
  methods: {
    toggleMneu(data) {
      this.mneuTab.forEach(item => {
        item.current = false;
      });
      data.current = true;
      this.model = data.type;
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  }
};
</script>
<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #344a5f;
}
.login_wrap {
  width: 330px;
  margin: auto;
}
.mneu_tab {
  text-align: center;
  li {
    display: inline-block;
    width: 88px;
    line-height: 36px;
    font-size: 14px;
    color: #fff;
    border-radius: 2px;
    cursor: pointer;
  }
  .current {
    background-color: rgba(0, 0, 0, 0.1);
  }
}
.login_form {
  margin-top: 29px;
  label {
    display: block;
    font-size: 14px;
    color: #fff;
    margin-bottom: 3px;
  }
  .item-form {
    margin-bottom: 13px;
  }
  .block {
    display: block;
    width: 100%;
  }
  .login-btn {
    margin-top: 19px;
  }
}
</style>