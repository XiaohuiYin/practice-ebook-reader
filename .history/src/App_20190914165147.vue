<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  mounted () {
    // 监听原生DOMContentLoaded事件, 确定页面基础字体大小
    document.addEventListener('DOMContentLoaded', this.fontSizeSet)
  },
  methods: {
    fontSizeSet () { // 设置页面基础字体大小
      const html = document.querySelector('html')
      let fontSize = window.innerWidth / 10
      fontSize = fontSize > 50 ? 50 : fontSize
      // console.log(fontSize)
      html.style.fontSize = fontSize + 'px'
    }
  }
}
</script>

<style lang="scss">

</style>
