<template>
  <div id="footer">
    <div class="col">
      <h5>About</h5>
      <p>My name is Shlomi and I’m a software engineer specializing in modern web development.</p>
    </div>
    <div class="col">
      <h5>Colophon</h5>
      <p>Built using <a href="https://nuxtjs.org/" target="_blank">Nuxt.js</a>, hosted on <a href="https://www.netlify.com/" target="_blank">Netlify</a>. This website sometimes works offline.</p>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator"

@Component
export default class Colophon extends Vue {}
</script>

<style lang="less" scoped>
@import url("../assets/less/style.less");

#footer {
    .text-size-tiny;
    line-height: 18px;

    margin: @64px 0px @64px 0px;

    display: grid;
    grid-template-columns: auto auto;
    border: 1px solid @color-gray-light;
    border-width: 1px 0 1px 0;

    .col {
        padding: @8px @24px;

        h5 {
            margin-top: @8px;
            margin-bottom: @4px;
        }
    }

    .col:first-child {
        border: 1px solid @color-gray-light;
        border-width: 0px 1px 0px 0px;
    }

    @media only screen and (max-width: @content-width) {
        margin: @48px 0px @48px 0px;
        grid-template-columns: auto;

        .col:first-child {
            border-width: 0px 0px 1px 0px;
        }

        .col {
          padding-left: 0px;
            padding-right: 0px;
        }
    }
}
</style>