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
  name: 'App'
  // methods:{
  //   DOMContentLoaded(){
  //     let fontSize =
  //   }
  // },
  // mounted(){

  // }
}
</script>

<style lang="scss">

</style>
