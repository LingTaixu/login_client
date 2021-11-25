<template>
  <div id="components-layout-demo-basic">
    <div class="main-header">
      <HeaderMain />
    </div>

    <div class="main">
      <Content />
    </div>

    <div class="main-footer">
      <Footer />
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
};
</script>

<style>
#components-layout-demo-basic {
  height: 100vh;
  display: flex;
  flex-direction: column;
}
.main-header {
  flex: 0.1;
}

.main {
  flex: 1;
}

.main-footer {
  flex: 0.1;
}
</style>
