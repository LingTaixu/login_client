<template>
  <div class="container">
    <div class="footer-main">
      <div class="footer-title">
        <a-row type="flex" justify="start">
          <a-col :span="4"> <span>首页</span> </a-col>
          <a-col :span="4"> <span @click="register">注册账号</span> </a-col>
          <a-col :span="4"> <span>官网</span> </a-col>
          <a-col :span="4"> <span>加入QQ群</span> </a-col>
        </a-row>
      </div>
      <div class="footer-login">
        <div @click="start" :class="btnClass">启动</div>
      </div>
    </div>
  </div>
</template>

<script>
// eslint-disable-next-line import/no-extraneous-dependencies
import { ipcRenderer } from 'electron';

export default {
  data() {
    return {
      btnClass: 'footer-login-btn-no',
    };
  },
  props: {
    loginStatus: {
      type: String,
    },
  },
  methods: {
    register() {
      this.$emit('register', 'register');
    },
    start() {
      const token = sessionStorage.getItem('token');
      ipcRenderer.send('start', token);
    },
  },
  watch: {
    loginStatus(data) {
      console.log(data);
      if (data === 'Yes') {
        this.btnClass = 'footer-login-btn';
      }
    },
  },
  created() {
    console.log(this.loginStatus);
  },
};
</script>

<style>
.container {
  background: url("../../assets/images/title.png");
  height: 100%;
  color: #fbbd8d;
}
.footer-main {
  display: flex;
}
.footer-title {
  flex: 1;
}
.footer-login {
  flex: 0.2;
}
.ant-col-4 {
  text-align: center;
  line-height: 60px;
  font-size: 18px;
}
.ant-col-4 > span {
  cursor: pointer;
}
.footer-login-btn {
  border: 1px dotted #40cce1;
  margin-top: 6px;
  height: 50px;
  width: 124px;
  color: #40cce1;
  text-align: center;
  font-size: 24px;
  line-height: 46px;
  background: #1c2023;
  cursor: pointer;
}

.footer-login-btn-no {
  border: 1px dotted #8B8B8B;
  margin-top: 6px;
  height: 50px;
  width: 124px;
  color: #8B8B8B;
  text-align: center;
  font-size: 24px;
  line-height: 46px;
  background: #1c2023;
  cursor:not-allowed;
  pointer-events:none;
}
</style>
