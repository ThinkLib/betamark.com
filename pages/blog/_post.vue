<template>
  <div id="blog-post">
    <div class="head">
      <h1 class="title">{{attributes.title}}</h1>
      <p class="date">{{attributes.date}}</p>
    </div>
    <div v-html="content"></div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'
import FrontMatter from 'front-matter'

import Prism from 'prismjs'
import 'prismjs/themes/prism.css'

declare module "vue" {
    interface Vue {
      attributes: any
    }
}

@Component({
  async asyncData({ params }) {
    const fileContent = await import(`~/static/posts/${params.post}.md`)
    const res = FrontMatter(fileContent.default)
    const MarkdownIt = require('markdown-it')({
      html: true,
      typographer: true,
      highlight: (str, lang) => {
        try {
          const className = 'language-js'
          return `<pre class="${className}"><code>${Prism.highlight(
            str,
            Prism.languages[lang],
            lang
          )}</code></pre>`
        } catch (__) {}
      }
    })

    return {
      attributes: res.attributes,
      content: MarkdownIt.render(res.body)
    }
  },
  head() {
    const _this: any = this;
    return {
      title: `Betamark - ${_this.attributes.title}`,
      meta: [
        { hid: 'description', name: 'description', content: _this.attributes.description }
      ]
    }
  }
})
export default class Post extends Vue {}
</script>

<style lang="less">
@import url("../../assets/less/style.less");

#blog-post {
  .head {
    text-align: center;
    margin-bottom: @32px;

    h1.title {
      margin-bottom: 0px;
    }

    p.date {
      .text-size-small;
      color: @color-gray-dark;
      margin-top: @32px;
    }
  } 
}

code[class*="language-"],
pre[class*="language-"] {
  font-size: 14px !important;
  line-height: 1.7 !important;
  text-shadow: none !important;
  margin: @24px 0px !important;
}

</style>