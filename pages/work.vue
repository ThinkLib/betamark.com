<template>
  <div>
    <h1>Work</h1>
    <h3><a href="https://www.yup.com" target="_blank">Yup</a></h3>
    <p>I'm currently working as a senior software engineer at Yup.com, an ed-tech company located in the Bay Area. My team and I create and maintain a collection of web applications written in modern JavaScript that provide tools for tutors and math students across the country. </p>

    <h3><a href="https://www.frontstand.com" target="_blank">Frontstand</a></h3>
    <p>Frontstand is a product I built while freelancing in Sydney. I designed and developed the website, web app and iOS app, and launched it in 2014. While it's no longer in active development, it's still being used by several companies in Australia and it's under ongoing maintenance.</p>

    <h3 class="projects">Open-source</h3>
      <div v-for="(project, index) in projects" class="project" :class="{first: index == 0}">
        <div class="title">
          <p><a :href="project.link" target="_blank">{{ project.title }}</a></p>
        </div>
        <div class="blurb">
          <p>{{ project.description }}</p> 
        </div>
      </div>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'

@Component({
  head() {
    return {
      title: `Betamark - Work`
    }
  }
})
export default class Work extends Vue {
  projects = [
    {
      title: "betamark.com",
      description: "Personal theme for Nuxt.js (source for this website).",
      link: "https://github.com/shlomnissan/betamark.com"
    },
    {
      title: "dave-wasm",
      description: "Remake of an old platform game in WebAssembly.",
      link: "https://github.com/shlomnissan/dave-wasm"
    },
    {
      title: "color-picker",
      description: "Natural color picker component for Vue.js.",
      link: "https://github.com/shlomnissan/vue-color-picker-board"
    },
    {
      title: "form-builder",
      description: "Drop-in form builder written in modern JavaScript.",
      link: "https://github.com/shlomnissan/formbuilder"
    }
  ]
  created() {

  }
}
</script>

<style lang="less" scoped>

@import url("../assets/less/style.less");

.project {
  display: grid;
  grid-template-columns: 130px auto;
  border-bottom: 1px solid @color-gray-light;

  &.first {
    margin-top: -@8px;
  }

  .title {
    display: flex;
    align-items: center;
  }

  &:last-child {
    border-bottom: 0px;
  }
}
</style>