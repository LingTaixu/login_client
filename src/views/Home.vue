<template>
  <div id="components-layout-demo-basic">
    <div class="main-header">
      <HeaderMain />
    </div>

    <div class="main">
      <Content :reg='reg' @closeReg="closeReg" @login='login'/>
    </div>

    <div class="main-footer">
      <Footer @register='register' :loginStatus='loginStatus' />
    </div>
  </div>
</template>

<script>
import HeaderMain from '@/views/components/header.vue';
import Content from '@/views/components/content.vue';
import Footer from '@/views/components/footer.vue';

export default {
  components: {
    HeaderMain,
    Footer,
    Content,
  },
  methods: {
    register(data) {
      this.reg = data;
    },
    closeReg(data) {
      this.reg = data;
    },
    login(data) {
      this.loginStatus = data;
    },
  },
  data() {
    return {
      reg: 'login',
      loginStatus: 'No',
    };
  },
};
</script>

<style>
#components-layout-demo-basic {
  height: 100vh;
  display: flex;
  flex-direction: column;
}
.main-header {
  flex: 0.03;
}
.main {
  flex: 1;
}
.main-footer {
  flex: 0.2;
}

</style>
