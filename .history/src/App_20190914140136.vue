<template>
  <div id="app">
    <span class="icon-back"></span>
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  mounted () {

  },
  methods: {
    fontSizeSet () {
      const html = document.querySelector('html')
      let fontSize = window.innerWidth / 10
      fontSize = fontSize > 50 ? 50 : fontSize
      console.log(fontSize)
      html.style.fontSize = fontSize + 'px'
    }
  }
}

window.onresize = () => {
  document.addEventListener('DOMContentLoaded', () => {

  })
}

</script>

<style lang="scss">

</style>
