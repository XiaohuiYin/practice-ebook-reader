<template>
  <div class='catalogue'>
    <transition name="slide">
      <div class="catalogue-wrappper">
        <ul class="catalogue-container">
          <li class="catalogue-item"
              v-for="(item,index) in navigation.doc"
              :key="index">
              {{item.label}}
          </li>
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
  },
  methods: {
    // ------------------------------------目录链接跳转
    jumpTo (data) {
      this.$emit('jumpTo', data)
    }
  }
}
</script>

<style lang='scss' scoped>
@import '@/assets/styles/global';

.catalogue{
  .catalogue-wrappper{
    .catalogue-container{
      .catalogue-item{

      }
    }
  }
}
</style>
