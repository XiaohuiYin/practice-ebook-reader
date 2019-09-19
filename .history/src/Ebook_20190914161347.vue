<template>
  <div class=''>
    aaaaaaaaaEbookaaaaaaa
  </div>
</template>

<script>
import Epub from 'epubjs' // 导入阅读器引擎
global.ePub = Epub
const DOWNLOAD_URL = '../public/ThePayPalWarsBattles.epub'

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
    // 电子书的解析和渲染
    showEpub () {
      // 1.生成book对象
      this.book = new Epub(DOWNLOAD_URL)
      console.log(this.book)

      // 2.生成 Rendition, 通过 book.renderTo ---renderTo(element, options)
      this.rendition = this.book.renderTo('read', { // 挂载点在id名为read的元素内
        width: window.innerWidth,
        height: window.innerHeight
      })

      // 3.通过 Rendition.display 渲染电子书
      this.rendition.display()
    }
  },
  mounted () {
    this.showEpub()
  }
}
</script>

<style lang='scss' scoped>
@import 'assets/styles/global'

</style>
