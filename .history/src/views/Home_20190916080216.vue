<template>
  <div class='home'>
    <router-link to="/ebook" tag="div">开始阅读吧!</router-link>
  </div>
</template>

<script>

export default {
  name: 'home'
}
</script>

<style lang='scss' scoped>
@import '@/assets/styles/global';

.home{
  padding-top: px2rem(220);
  font-size: px2rem(80);
  font-weight: bold;
  text-align: center;
  color: aqua;
  background-color: #f66;
  cursor: pointer;
}
</style>
