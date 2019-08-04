<template lang="pug">
.container
  .raw(v-if="show === 'md'") {{readme.raw}}
  .raw(v-if="show === 'html'") {{readme.html}}
  mpParse(v-else-if="readme.html", :content="readme.html", ref="mpParse")
  .turn-btn(@click="turn") 切换
</template>

<script>
import marked from 'marked'
import mpParse from 'mpvue-mpParse'

export default {
  components: {
    mpParse
  },
  data () {
    return {
      types: ['rich', 'md', 'html'],
      show: 'rich',
      readme: {
        raw: '',
        html: ''
      }
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
      const res = await this.$request.get('https://gitlab.com/F-loat/mpvue-mpParse/raw/master/README.md')
      this.readme.raw = res.data
      this.readme.html = marked(res.data)
      mpvue.stopPullDownRefresh()
      mpvue.hideLoading()
      console.log(this.$refs.mpParse.nodes)
    },
    turn () {
      const current = this.show
      const currentIndex = this.types.indexOf(current)
      if (currentIndex < this.types.length) {
        this.show = this.types[currentIndex + 1]
      } else {
        this.show = this.types[0]
      }
    }
  }
}
</script>

<style>
@import url("~mpvue-mpParse/src/mpParse.css");
</style>
