<template>
  <div class='home'>
    <router-link to="/ebook" >开始阅读吧!</router-link>
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
  color: aqua;
  text-align: center;
}
</style>
