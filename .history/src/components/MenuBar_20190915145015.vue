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
          <div class="preview" :style="{fontSize: minFontSize + 'px', left: offsetA + 'px'}">A</div>
          <div class="select-wrapper" ref="fontSelect">
            <div class="select" v-for="(item, index) in fontSizeList" :key="index">
              <div class="line"></div>
              <div class="dot"></div>
              <div class="line"></div>
            </div>
          </div>
          <div class="preview" :style="{fontSize: maxFontSize + 'px', right: offsetA + 'px'}">A</div>
        </div>
      </div>
    </transition>
  </div>

</template>

<script>

export default {
  name: 'menu-bar',
  props: {
    ifTitleAndMenuShow: { // 判断 标题和菜单 显隐
      type: Boolean,
      default: false
    },
    fontSizeList: { // 字体大小可供选择组
      type: Array
    },
    defaultFontSize: { // 默认字体小
      type: Number,
      default: 16
    }
  },
  data () {
    return {
      ifSettingShow: false, // 判断 配置栏 显隐
      offsetA: 0 // 使 A 居中需要的相对位移
    }
  },
  methods: {
    showSetting () {
      this.ifSettingShow = true
      this.centerA()
    },
    hideSetting () {
      this.ifSettingShow = false
    },
    centerA () {
      // 字号选择区总宽度÷字号选择个数 = 每个选择块的宽度
      // 使 "A" 居中需要的relative位移 = 每个选择块的宽度的四分之一(数学知识)
      this.$nextTick(function () {
        let eachSelectWidth = this.$refs.fontSelect.clientWidth / this.fontSizeList.length
        console.log(this.$refs.fontSelect.clientWidth)
        console.log(eachSelectWidth)
        this.offsetA = eachSelectWidth / 4
      })
    }
  },
  computed: {
    minFontSize () { // 最小字号
      return this.fontSizeList[0].fontSize
    },
    maxFontSize () { // 最大字号
      return this.fontSizeList[this.fontSizeList.length - 1].fontSize
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
      .preview{
        flex: 0 0 px2rem(40);
        @include center();
        position: relative;
        background: green;
      }
      .select-wrapper{
        flex:1;
        display: flex;
        justify-content: space-between;
        .select{
          flex: 1;
          display: flex;
          @include center();
          height: 100%;
          &:nth-of-type(1){
            .line{
              &:nth-of-type(1){
                visibility: hidden;
              }
            }
          }
          &:nth-last-of-type(1){
            .line{
              &:nth-last-of-type(1){
                visibility: hidden;
              }
            }
          }
          .line{
            flex: 1;
            height: 0;
            border-top: px2rem(1) solid #ccc;
          }
          .dot{
            flex: 0 0 0;
            height: px2rem(8);
            border-right: px2rem(1) solid #ccc;
          }
        }
      }
    }
  }
}

</style>
