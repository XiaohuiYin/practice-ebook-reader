<template>
  <div class="menu-bar" ref="menuBar">
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
        <div class="setting-theme" v-else-if="curSetting===2">
          <div class="themes-wrapper">
            <div class="each-theme"
                 v-for="(item, index) in themesList"
                 :key="index"
                 @click="setTheme(index)">
                 <div class="sketch"
                      :class="{'no-border': item.name!=='default'}"
                      :style="{background: item.style.body.background}"></div>
                 <div class="name" :class="{'active-name': defaultTheme===index}">{{item.name}}</div>
            </div>
          </div>
        </div>

        <!-- 进度设置 -->
        <div class="setting-progress" v-else-if="curSetting===1">
          <div class="progress-wrapper">
            <input class="progress"
                   type="range"
                   max="100"
                   min="0"
                   step="1"
                   @change="onProgressChange($event.target.value)"
                   @input="onProgressInput($event.target.value)"
                   :value="progress"
                   :disabled="!ifBookAvailable"
                   ref="progress">
          </div>
          <div class="text-wrapper">
            <span>{{ifBookAvailable ? progress + '%' : '加载中...'}}</span>
          </div>
        </div>

        <!-- 菜单设置 -->
        <div class="setting-menu" v-else></div>
      </div>
    </transition>

    <!-- 目录弹出层 -->

    <!-- 目录弹出层遮罩 -->
    <transition name="fade">
      <div class="content-mask"
           v-show="ifShowContent"
           @click="hideContent">

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
    },
    ifBookAvailable: {
      type: Boolean
    },
    navigation: {
      type: Object
    }
  },
  data () {
    return {
      ifSettingShow: false, // 判断 配置栏 显隐
      offsetA: 0, // 使 A 居中需要的相对位移
      curSetting: -1, // 当前设置栏选项卡
      progress: 0, // 当前进度 0-100
      ifShowContent: false  //默认不显示目录
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
        let eachSelectWidth = this.$refs.fontSelect.offsetWidth / this.fontSizeList.length
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
    },
    // ------------------------------------拖动进度条时触发事件
    onProgressInput (curProgress) {
      this.progress = curProgress // 实时更新当前进度
      this.$refs.progress.style.backgroundSize = `${curProgress}% 100%` // 通过background-size动态改变进度条左右两部分背景颜色
    },
    // ------------------------------------放开进度条后触发事件, 根据进度条progress数值跳转到相应页面
    onProgressChange (curProgress) {
      this.$emit('onProgressChange', curProgress)
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
    z-index: 102;
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
    bottom: px2rem(47);
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
    .setting-theme{
      display: flex;
      width: 100%;
      height: 100%;
      color: #ccc;
      .themes-wrapper{
        flex:1;
        display: flex;
        .each-theme{
          flex: 1;
          display: flex;
          flex-direction: column;
          padding: px2rem(10) px2rem(10) 0;
          box-sizing: border-box;
          .sketch{
            flex:1;
            border: px2rem(1) solid #999;
            &.no-border{
              border: none;
            }
          }
          .name{
            height: px2rem(26);
            font-size: px2rem(14);
            text-align: center;
            line-height: px2rem(26);
            &.active-name{
              color:#222;
            }
          }
        }
      }
    }
    .setting-progress{
      width:100%;
      height: 100%;
      .progress-wrapper{
        width: 100%;
        height: 60%;
        @include center();
        padding: 0 px2rem(30);
        box-sizing: border-box;
        .progress{
          -webkit-appearance: none;
          outline: none;
          width: 100%;
          height: px2rem(2);
          background: -webkit-linear-gradient(#999, #999) no-repeat, #ddd;
          background-size: 0 100%;
          &::-webkit-slider-thumb{
            -webkit-appearance: none;
            width: px2rem(20);
            height: px2rem(20);
            border-radius: 50%;
            border: px2rem(1) solid #ddd;
            background-color: #fff;
            box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, .15)
          }
        }
      }
      .text-wrapper{
        width: 100%;
        height: 40%;
        @include center();
        font-size: px2rem(14);
        color: #222;
      }
    }
  }
}

</style>
