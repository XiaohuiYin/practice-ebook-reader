<template>
  <div class="menu-bar">
    <!-- 主菜单 -->
    <transition name="slide-up">
      <div class="menu-wrapper"
           :class="{'hide-box-shadow':!ifTitleAndMenuShow || ifSettingShow}"
           v-show="ifTitleAndMenuShow">
          <div class="icon-wrapper"
               v-for="(item, index) in menusList"
               :key="index">
            <span :class="item.name"
                  class="icon"
                  @click="showSetting(index)">{{item.text}}
            </span>
          </div>
      </div>
    </transition>

    <!-- 设置栏 -->
    <transition name="slide-up">
      <div class="setting-wrapper" v-show="ifSettingShow">
        <!-- 字体设置 -->
        <div class="setting-font-size" v-if="curSetting===3">
          <div class="preview" :style="{fontSize: minFontSize + 'px', left: offsetA + 'px'}">A</div>
          <div class="select-wrapper" ref="fontSelect">
            <div class="select" v-for="(item, index) in fontSizeList" :key="index" @click="setFontSize(item.fontSize)">
              <div class="line"></div>
              <div class="dot-wrapper">
                <div class="dot" v-show="defaultFontSize === item.fontSize">
                  <div class="small-dot"></div>
                </div>
              </div>
              <div class="line"></div>
            </div>
          </div>
          <div class="preview" :style="{fontSize: maxFontSize + 'px', right: offsetA + 'px'}">A</div>
        </div>

        <!-- 主题设置 -->
        <div class="setting-theme" v-else-if="curSetting===2"></div>

        <!-- 进度设置 -->
        <div class="setting-progress" v-else-if="curSetting===1"></div>

        <!-- 菜单设置 -->
        <div class="setting-more" v-else-if="curSetting===1"></div>
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
    menusList: { // 菜单栏选项卡
      type: Array
    },
    fontSizeList: { // 字体大小可供选择组
      type: Array
    },
    defaultFontSize: { // 默认字体小
      type: Number,
      default: 16
    },
    themesList: { // 主题列表
      type: Array
    },
    defaultTheme: { // 默认主题样式(本项目设为了护眼模式：eye)
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      ifSettingShow: false, // 判断 配置栏 显隐
      offsetA: 0, // 使 A 居中需要的相对位移
      curSetting: -1 // 当前设置栏选项卡
    }
  },
  methods: {
    // ------------------------------------显示 设置栏
    showSetting (settingIndex) {
      this.ifSettingShow = true
      this.curSetting = settingIndex
      this.curSetting === 3 && this.centerA()
    },
    // ------------------------------------隐藏 设置栏
    hideSetting () {
      this.ifSettingShow = false
    },
    // ------------------------------------使设置栏 左右的字母 "A" 居中
    centerA () {
      // 字号选择区总宽度÷字号选择个数 = 每个选择块的宽度
      // 使 "A" 居中需要的relative位移 = 每个选择块的宽度的四分之一(数学知识)(要在css里事先定义好position:relative)
      // 一定要用 $nextTick
      this.$nextTick(() => {
        let eachSelectWidth = this.$refs.fontSelect.clientWidth / this.fontSizeList.length
        // console.log(this.$refs.fontSelect.clientWidth)
        // console.log(eachSelectWidth)
        this.offsetA = eachSelectWidth / 4
      })
    },
    // ------------------------------------改变 字体大小
    setFontSize (fontSize) {
      this.$emit('setFontSize', fontSize)
    },
    // ------------------------------------改变 主题样式
    setTheme (themeIndex) {
      this.$emit('setTheme', themeIndex)
    }
  },
  computed: {
    minFontSize () { // 最小字号
      return this.fontSizeList[0].fontSize
    },
    maxFontSize () { // 最大字号
      return this.fontSizeList[this.fontSizeList.length - 1].fontSize
    }
  },
  mounted () {
    let _this = this
    window.onresize = () => { // 为了适配PC端 改变窗口大小时也能适时"居中"显示
      _this.centerA()
    }
  },
  destroyed () { // 由于window.onresize是全局事件，在其他页面改变界面时也会执行，这样可能会出现问题，需要在出这个界面时注销window.onresize事件
    window.onresize = null
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
        // background: green;
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
          .dot-wrapper{
            position: relative;
            flex: 0 0 0;
            width: 0;
            height: px2rem(7);
            border-right: px2rem(1) solid #ccc;
            .dot{
              position: absolute;
              top: px2rem(-8);
              left: px2rem(-10);
              width: px2rem(20);
              height: px2rem(20);
              border-radius: 50%;
              border: px2rem(1) solid #ccc;
              box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, .15);
              background-color: #fff;
              @include center();
              .small-dot{
                width: px2rem(5);
                height: px2rem(5);
                border-radius: 50%;
                background-color: #000;
              }
            }
          }
        }
      }
    }
  }
}

</style>
