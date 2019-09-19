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

    }
  },
  methods: {
    // 电子书的解析和渲染
    showEpub () {

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
