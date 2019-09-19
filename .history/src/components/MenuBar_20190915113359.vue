<template>
  <div class="menu-bar">
    <!-- 主菜单 -->
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

    <!-- 设置栏 -->
    <transition name="slide-up">
      <div class="setting-wrapper" v-show="ifSettingShow">
        <!-- 字体设置 -->
        <div class="setting-font-size">
          <div class="preview">A</div>
          <div class="select-wrapper">
            <div class="select" v-for="(item, index) in fontSizeList" :key="index">
              <div class="line"></div>
              <div class="dot"></div>
              <div class="line"></div>
            </div>
          </div>
          <div class="preview">A</div>
        </div>
      </div>
    </transition>
  </div>

</template>

<script>

export default {
  name: 'menu-bar',
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    },
    fontSizeList: {
      type: Array
    },
    defaultFontSize: {
      type: Number,
      default: 16
    }
  },
  data () {
    return {
      ifSettingShow: false
    }
  },
  methods: {
    showSetting () {
      this.ifSettingShow = true
    },
    hideSetting () {
      this.ifSettingShow = false
    }
  },
  computed: {
    miniFontSize () {

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
  }
  .setting-wrapper{
    position: absolute;
    bottom: px2rem(48);
    left: 0;
    z-index: 101;
    width: 100%;
    height: px2rem(60);
    background-color: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
    .setting-font-size{
      display: flex;
      width: 100%;
      height: 100%;
      background-color: red;
      .preview{
        flex: 0 0 px2rem(40);
        @include center();
        background: green;
      }
      .select-wrapper{
        flex:1;
        background-color: yellow;
        .select{
          .line{

          }
          .dot{

          }
        }
      }
    }
  }
}

</style>
