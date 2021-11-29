<template>
  <div class="Header">
    <div class="title">欢迎来到黑纱丝·柯塞尔</div>
    <div class="content"></div>
    <div class="foot">
      <a-icon type="minus" @click="windowMin" style="font-size: 20px; cursor: pointer" />
      <a-icon
        type="close"
        @click="windowClose"
        style="font-size: 22px; margin-left: 4px; cursor: pointer"
      />
    </div>
  </div>
</template>

<script>
// eslint-disable-next-line import/no-extraneous-dependencies
import { ipcRenderer } from 'electron';

export default {
  methods: {
    windowMin() {
      // ipcRenderer.send('window-min');
    },
    windowClose() {
      ipcRenderer.send('guanBi');
    },
  },
};
</script>

<style>
@font-face {
  font-family: myFirstFont;
  src: url("../../assets/fonts.TTF");
}
.Header {
  display: flex;
  background: url("../../assets/images/title.png");
  -webkit-user-select: none;
  padding: 0px 10px;
}
.title {
  flex: 0.5;
  font-family: myFirstFont;
  font-weight: bold;
  font-size: 22px;
  color: #cc9a74;
}

.content {
  flex: 1;
  font-family: myFirstFont;
  text-align: center;
  line-height: 30px;
  font-size: 20px;
  color: #f6ba8b;
}
.foot {
  flex: 0.14;
  text-align: center;
  line-height: 33px;
  color: #f6ba8b;
}
</style>
