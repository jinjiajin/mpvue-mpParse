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
      const list = await this.$request.get('https://news-at.zhihu.com/api/4/news/latest')
      const article = list.data.stories[0] || {}
      const detail = await this.$request.get(`https://news-at.zhihu.com/api/4/news/${article.id}`)
      this.article = detail.data.body
      mpvue.stopPullDownRefresh()
      mpvue.hideLoading()
      setTimeout(() => console.log(this.$refs.mpParse.nodes), 1000)
    }
  }
}
</script>

<style>
@import url("~mpvue-mpParse/src/mpParse.css");
</style>
