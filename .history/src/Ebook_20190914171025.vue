<template>
  <div class='ebook'>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center"></div>
        <div class="left" @click="nextPage"></div>
      </div>
    </div>
  </div>
</template>

<script>
import Epub from 'epubjs' // 导入阅读器引擎
// global.ePub = Epub
const DOWNLOAD_URL = '/ThePayPalWarsBattles.epub' // PS: 此处路径需注意, 放在public文件夹内, 写url的时候不要写../public

export default {
  name: 'ebook',
  props: {

  },
  data () {
    return {
      book: {},
      rendition: {}
    }
  },
  methods: {
    // ----------------------电子书的解析和渲染
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
    },
    // ---------------电子书上一页
    prevPage () { // Rendition.prev
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    // ---------------电子书下一页
    nextPage () { // Rendition.next
      if (this.rendition) {
        this.rendition.next()
        console.log(this.rendition)
      }
    }
  },
  mounted () {
    this.showEpub()
  }
}
</script>

<style lang='scss' scoped>
@import 'assets/styles/global';

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
        background-color: #4f2;
        opacity: .2;
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
