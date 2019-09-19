<template>
  <div class='ebook'>
    <!-- 阅读器 标题栏 -->
    <TitleBar :ifTitleAndMenuShow="ifTitleAndMenuShow"></TitleBar>

    <!-- 阅读器 阅读主体 -->
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTitleAndMenu"></div>
        <div class="left" @click="nextPage"></div>
      </div>
    </div>

    <!-- 阅读器 菜单栏 -->
    <MenuBar :ifTitleAndMenuShow="ifTitleAndMenuShow"  ref="menuBar" :fontSizeList="fontSizeList" :defaultFontSize="defaultFontSize" @setFontSize="setFontSize"></MenuBar>
  </div>
</template>

<script>
import Epub from 'epubjs' // 导入阅读器引擎
import TitleBar from '@/components/TitleBar.vue' // 导入组件
import MenuBar from '@/components/MenuBar.vue' // 导入组件
// global.ePub = Epub

// 定义常量, 存储本地资源里的epub电子书路径, 以便epub.js解析
// PS: 此处路径需注意, 放在public文件夹内, 写url的时候不要写../public
const DOWNLOAD_URL = '/HistoryofChinesePainting.epub'

export default {
  name: 'ebook',
  data () {
    return {
      book: {}, // 储存 epub.book对象
      rendition: {}, // 储存 epub.book.rendition对象
      themes: {}, // 储存epub.book.rendition.themes对象
      ifTitleAndMenuShow: false, // 控制 标题和菜单栏 显示/隐藏
      defaultFontSize: 16, // 默认选中字体大小
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ] // 设置字体大小数组
    }
  },
  components: {
    TitleBar,
    MenuBar
  },
  methods: {
    // --------------------------------电子书的解析和渲染--------------------------------
    showEpub () {
      // 1.生成book对象
      this.book = new Epub(DOWNLOAD_URL)
      // console.log(this.book)

      // 2.生成 Rendition, 通过 book.renderTo ---renderTo(element, options)
      this.rendition = this.book.renderTo('read', { // 电子书挂载点在id名为read的元素内
        width: window.innerWidth,
        height: window.innerHeight
      })

      // 3.通过 Rendition.display 渲染电子书
      this.rendition.display()

      // 4.获取themes对象, 以便改变阅读样式
      this.themes = this.rendition.themes
      this.themes.fontSize(this.defaultFontSize + 'px')
    },
    // ---------------------------------电子书上一页--------------------------------
    prevPage () { // Rendition.prev
      this.rendition && this.rendition.prev()
    },
    // ---------------------------------电子书下一页--------------------------------
    nextPage () { // Rendition.next
      this.rendition && this.rendition.next()
    },
    toggleTitleAndMenu () {
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
      !this.ifTitleAndMenuShow && this.$refs.menuBar.hideSetting()
    },
    // ---------------------------------监听menu子组件点击事件切换字体大小 通过epub.book.renditin提供的themes对象--------------------------------
    setFontSize (fontSize) {
      this.defaultFontSize = fontSize
      this.themes && this.themes.fontSize(fontSize * 1)
    }
  },
  mounted () {
    this.showEpub()
  }
}
</script>

<style lang='scss' scoped>
@import '@/assets/styles/global';

.ebook{
  position: relative;
  .read-wrapper{
    .mask{
      position: absolute;
      top: 0;
      left: 0;
      z-index: 100;
      display: flex;
      width: 100%;
      height: 100%;
      .left,.rigth{
        flex: 0 0 px2rem(100);
        // background-color: #4f2;
        // opacity: .2;
      }
      .center{
        flex: 1;
        // background-color: #62d;
        // opacity: .2;
      }
    }
  }
}
</style>
