<template>
  <div class=''>
    aaaaaaaaaEbookaaaaaaa
  </div>
</template>

<script>
import Epub from 'epubjs' // 导入阅读器引擎

const DOWNLOAD_URL = '../public/ThePayPalWarsBattles.epub'

export default {
  name: 'ebook',
  props: {

  },
  data () {
    return {
      book : {}
    }
  },
  methods: {
    // 电子书的解析和渲染
    showEpub () {
      // 1.生成book对象
      this.book = 
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
