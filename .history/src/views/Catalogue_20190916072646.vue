<template>
  <div class='catalogue'>
    <transition name="slide">
      <div class="catalogue-wrappper">
        <ul class="catalogue-container">
          <li class="catalogue-item"></li>
        </ul>
      </div>
    </transition>
  </div>
</template>

<script>

export default {
  name: 'catalogue',
  props: {
    navigation: Object
  },
  data () {
    return {

    }
  }
}
</script>

<style lang='scss' scoped>
@import '@/assets/styles/global';

</style>
