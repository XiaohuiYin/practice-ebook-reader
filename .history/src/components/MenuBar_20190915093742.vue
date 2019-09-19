<template>
  <transition name="slide-up">
    <div class="menu-wrapper" v-show="ifTitleAndMenuShow">
        <div class="icon-wrapper">
          <span class="icon-menu icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-progress icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-bright icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-a icon">A</span>
        </div>
    </div>
  </transition>
</template>

<script>

export default {
  name: '',
  props: {

  },
  data () {
    return {

    }
  }
}
</script>

<style lang='scss' scoped>

</style>
