<template>
  <div class="menu-bar">
    <transition name="slide-up">
      <div class="menu-wrapper" :class="{'hide-box-shadow':!ifTitleAndMenuShow || ifSettingShow}" v-show="ifTitleAndMenuShow">
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
            <span class="icon-a icon" @click="showSetting">A</span>
          </div>
      </div>
    </transition>

    <div class="setting-wrapper" v-show="ifSettingShow">
      <div class="setting-font-size"></div>
    </div>
  </div>

</template>

<script>

export default {
  name: 'menu-bar',
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      ifSettingShow: false
    }
  },
  methods: {
    showSetting () {
      this.ifSettingShow = !this.ifSettingShow
    }
  }
}
</script>

<style lang='scss' scoped>
@import '@/assets/styles/global';

.menu-bar{
  .menu-wrapper{
    position: absolute;
    bottom: 0;
    left: 0;
    z-index: 101;
    display: flex;
    width: 100%;
    height: px2rem(48);
    font-size: px2rem(20);
    background-color: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
    &.hide-box-shadow{
      box-shadow: none;
    }
    .icon-wrapper{
      flex: 1;
      @include center();
      .icon-progress{
        font-size: px2rem(28);
      }
      .icon-bright{
        font-size: px2rem(24);
      }
    }
    &.slide-up-enter, &.slide-up-leave-to{
      transform: translate3d(0, 100%, 0);
    }
    &.slide-up-enter-to, &.slide-up-leave{
      transform: translate3d(0, 0, 0);
    }
    &.slide-up-enter-active, &.slide-up-leave-active{
      transition: all .3s linear;
    }
  }
  .setting-wrapper{
    .setting-font-size{

    }
  }
}

</style>
