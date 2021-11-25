<template>
  <div class="Header">
    <a-button @click="start">登录</a-button>
  </div>
</template>

<script>

const exec = require('child_process');

export default {
  methods: {
    start() {
      console.log(exec);
    },
  },
};
</script>

<style>
</style>
