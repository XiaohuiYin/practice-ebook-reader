<template>
  <transition name="slide">
    <div class='catalogue'>
        <div class="catalogue-wrappper">
          <ul class="catalogue-container">
            <li class="catalogue-item"
                v-for="(item,index) in navigation.toc"
                :key="index"
                @click="jumpTo(item.href)">
                {{item.label}}
            </li>
            <li class="catalogue-item"
                v-for="(item,index) in navigation.toc"
                :key="index"
                @click="jumpTo(item.href)">
                {{item.label}}
            </li>
          </ul>
        </div>
    </div>
  </transition>
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
  position: fixed;
  top: 0;
  left: 0;
  z-index: 110;
  width: 80%;
  height: 100%;
  background-color: white;
  overflow-y: auto;
  .catalogue-wrappper{
    .catalogue-container{
      padding: px2rem(30) px2rem(50);
      list-style: none;
      .catalogue-item{
        padding: px2rem(20) 0 px2rem(10);
        font-size: px2rem(16);
        list-style: none;
        border-bottom: 1px solid #ddd;
        cursor: pointer;
      }
    }
  }
}
</style>
