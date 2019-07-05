<template>
  <div id="home">
    <img class="hero" src="/hero.jpg" alt="Shlomi Nissan - website hero" />
    <p>I'm writing, thinking and talking about modern web development: how to build performant web apps, how they scale, and how they are being maitnaned.</p>
    <p>I’m currently living in San Francisco and working as a senior software engineer at Yup where I help to create and maintain applications that provide tools for online education.</p>
    <p>I’m currently unavailable for work but I’m always happy to hear about new ideas so feel free to <a href="https://twitter.com/shlominissan" target="_blank">reach out</a>.</p>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'

@Component({
  head() {
    return {
      title: 'Betamark - Shlomi Nissan'
    }
  }
})
export default class Index extends Vue {}
</script>

<style lang="less" scoped>
@import url("../assets/less/style.less");
#home {
  .hero {
    margin-bottom: @8px;
  }
}
</style>