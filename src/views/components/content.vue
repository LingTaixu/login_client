<template>
  <div class="container">
    <a-carousel class="main" autoplay>
      <img src="../../assets/images/bg_1.jpg" />
      <img src="../../assets/images/bg_2.jpg" />
      <img src="../../assets/images/bg_3.jpg" />
      <img src="../../assets/images/bg_4.jpg" />
      <img src="../../assets/images/bg_5.jpg" />
      <img src="../../assets/images/bg_6.jpg" />
      <img src="../../assets/images/bg_7.jpg" />
      <img src="../../assets/images/bg_8.jpg" />
    </a-carousel>
    <a-divider class="test" style="color: #fff">黑纱丝 • 第三季 • 科赛尔版本</a-divider>
    <div class="loginBox" v-if="reg === 'login' && loginStatus">
      <a-form-model
        ref="login"
        :rules="loginRules"
        :model="login"
        :label-col="labelCol"
        :wrapper-col="wrapperCol"
      >
        <a-form-model-item label="账号" prop="Account">
          <a-input v-model="login.Account" />
        </a-form-model-item>
        <a-form-model-item label="密码" prop="Password">
          <a-input type="password" v-model="login.Password" />
        </a-form-model-item>

        <a-form-model-item :wrapper-col="{ span: 14, offset: 7 }">
          <button class="loginBtn" type="primary" @click="onLogin">登 录</button>
          <a-checkbox @change="onChange">记住我</a-checkbox>
        </a-form-model-item>
      </a-form-model>
    </div>

    <!-- 注册 -->
    <div class="registerBox" v-show="reg === 'register' && loginStatus">
      <a-form-model
        ref="registerName"
        :rules="registerRules"
        :model="register"
        :label-col="labelCol"
        :wrapper-col="wrapperCol"
      >
        <a-form-model-item label="账号" ref="Account" prop="Account">
          <a-input
            v-model="register.Account"
            @blur="
              () => {
                $refs.Account.onFieldBlur();
              }
            "
          />
        </a-form-model-item>
        <a-form-model-item label="密码" ref="Password" prop="Password">
          <a-input type="password" v-model="register.Password" />
        </a-form-model-item>
        <a-form-model-item label="确认密码" ref="towPassword" prop="towPassword">
          <a-input type="password" v-model="register.towPassword" />
        </a-form-model-item>
        <a-form-model-item label="邮箱" prop="userEmail">
          <a-input v-model="register.userEmail" />
        </a-form-model-item>
        <a-form-model-item :wrapper-col="{ span: 14, offset: 8 }">
          <button class="loginBtn" type="primary" @click="onRegister('registerName')">
            注册账号
          </button>
          <button class="loginBtn" type="primary" @click="onClose">取消</button>
        </a-form-model-item>
      </a-form-model>
    </div>

    <div class="registerBox" v-if="!loginStatus">
      欢迎你 {{login.Account}}
    </div>
  </div>
</template>

<script>
/* eslint-disable consistent-return */
import axios from 'axios';
import qs from 'qs';

export default {
  data() {
    const validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码！'));
        this.pwd = false;
      } else if (this.register.towPassword) {
        this.$refs.registerName.validateField('towPassword');
      } else if (value) {
        this.pwd = true;
      }
    };
    const validatePass2 = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请确认密码！'));
        this.pwdT = false;
      } else if (value !== this.register.Password) {
        callback(new Error('两次输入密码不同'));
        this.pwdT = false;
      } else {
        this.pwdT = true;
        callback();
      }
    };
    // eslint-disable-next-line consistent-return
    const userEmail = (rule, value, callback) => {
      const mailReg = /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(.[a-zA-Z0-9_-])+/;
      if (!value) {
        this.EmailFlag = false;
        return callback(new Error('邮箱不能为空'));
      }
      setTimeout(() => {
        if (mailReg.test(value)) {
          this.EmailFlag = true;
          callback();
        } else {
          callback(new Error('请输入正确的邮箱格式'));
          this.EmailFlag = false;
        }
      }, 100);
    };
    return {
      labelCol: { span: 8 },
      wrapperCol: { span: 14 },
      login: {},
      register: {},
      pwd: false,
      pwdT: false,
      EmailFlag: false,
      checkedReadMe: false,
      loginStatus: true,
      userInfo: {},
      registerRules: {
        Account: [
          { required: true, message: '请输入账号！', trigger: 'blur' },
          {
            min: 3,
            max: 12,
            message: '请输入3到12位之间的账号',
            trigger: 'blur',
          },
        ],
        Password: [
          { required: true, message: '请输入密码！', trigger: 'blur' },
          { validator: validatePass, trigger: 'change' },
        ],
        towPassword: [{ validator: validatePass2, trigger: 'change' }],
        userEmail: [{ validator: userEmail, trigger: 'change' }],
      },
      loginRules: {
        Account: [{ required: true, message: '请输入账号！', trigger: 'blur' }],
        Password: [{ required: true, message: '请输入密码！', trigger: 'blur' }],
      },
    };
  },
  methods: {
    onLogin() {
      this.$refs.login.validate((valid) => {
        if (valid) {
          const params = {
            Account: this.login.Account,
            Password: this.login.Password,
          };
          console.log(params);
          axios
            .post('http://localhost:8322/account/login', qs.stringify(params))
            .then((response) => {
              console.log(response);
              if (response.status === 200) {
                if (response.data.IsSucceed) {
                  this.$message.success({ content: response.data.Message, key: '1', duration: 5 });
                  sessionStorage.setItem('token', response.data.data.token);
                  this.$emit('login', 'Yes');
                  this.loginStatus = false;
                  this.userInfo = response.data.data;
                } else {
                  this.$message.error({
                    content: response.data.Message,
                    key: '1',
                    duration: 5,
                    top: '100',
                  });
                }
              }
            })
            .catch((error) => {
              console.log(error);
            });
        }
      });
    },
    onChange(e) {
      this.checkedReadMe = e.target.checked;
    },
    onRegister() {
      if (!this.register.Account) {
        this.$refs.registerName.validateField('Account');
        return;
      }
      if (!this.register.Password) {
        this.$refs.registerName.validateField('Password');
        return;
      }
      if (!this.register.towPassword) {
        this.$refs.registerName.validateField('towPassword');
        return;
      }
      if (!this.register.userEmail) {
        this.$refs.registerName.validateField('userEmail');
        return;
      }
      console.log(this.pwd, this.pwdT, this.EmailFlag);
      if (this.pwd && this.pwdT && this.EmailFlag) {
        this.$message.loading({ content: '注册中...', key: '1' });
        const params = {
          Account: this.register.Account,
          Password: this.register.Password,
          Email: this.register.userEmail,
        };
        axios
          .post('http://localhost:8322/account/register', qs.stringify(params))
          .then((response) => {
            console.log(response);
            if (response.status === 200) {
              if (response.data.IsSucceed) {
                this.$message.success({ content: response.data.Message, key: '1', duration: 5 });
              } else {
                this.$message.error({
                  content: response.data.Message,
                  key: '1',
                  duration: 5,
                  top: '100',
                });
              }
            }
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    onClose() {
      this.$refs.registerName.resetFields();
      this.$emit('closeReg', 'login');
    },
  },
  props: {
    reg: {
      type: String,
    },
  },
  watch: {
    reg(data) {
      console.log(data, 'content');
    },
  },
  created() {
    const checkedReadMe = localStorage.getItem('checkedReadMe');
    if (checkedReadMe) {
      this.checkedReadMe = checkedReadMe;
    }
  },
};
</script>

<style>
img {
  height: 450px;
}

.main {
  position: relative;
}
.test {
  position: absolute !important;
  top: 366px !important;
}

.loginBox {
  position: absolute !important;
  top: 134px;
  left: 10px;
  width: 300px;
  height: 100px;
  text-align: center;
}
label {
  color: #bb9874 !important;
}
.ant-form-item {
  margin-bottom: 8px !important;
}
.loginBtn {
  height: 30px;
  outline: none;
  line-height: 24px;
  margin-right: 20px;
  border: none !important;
  cursor: pointer;
  background: rgba(187, 152, 116, 0);
  color: #bb9874;
}
.registerBox {
  position: absolute !important;
  top: 104px;
  left: 10px;
  width: 300px;
  height: 100px;
  text-align: center;
}
.ant-form-item-required::before {
  content: "" !important;
}
</style>
