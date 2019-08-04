<template lang="pug">
.container
  .raw(v-if="raw") {{article}}
  mpParse(v-else, :content="article", ref="mpParse")
  .turn-btn(@click="raw = !raw") 切换
</template>

<script>
import mpParse from 'mpvue-mpParse'

export default {
  components: {
    mpParse
  },
  data () {
    return {
      raw: false,
      article: ''
    }
  },
  mounted () {
    this.getData()
  },
  onPullDownRefresh () {
    this.getData()
  },
  methods: {
    async getData () {
      mpvue.showLoading({ title: '加载中' })
      const article = await this.$request.get('https://m.dealmoon.com/cn/15-Off-Sitewide-Sephora-8-5/858287.html')
      this.article = article.data
      mpvue.stopPullDownRefresh()
      mpvue.hideLoading()
    }
  }
}
</script>

<style>
@import url("~mpvue-mpParse/src/mpParse.css");
</style>
