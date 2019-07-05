<template>
  <div id="website-container">
    <navigation></navigation>
      <div :class="{post: isPost}" id="content-container">
        <nuxt/>
      </div>
    <colophon></colophon>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator"
import Navigation from "~/components/Navigation.vue"
import Colophon from "~/components/Colophon.vue"

@Component({
    components: {
        Navigation,
        Colophon
    }
})
export default class Default extends Vue {
  get isPost() {
    return this.$route.name === "blog-post"
  } 
}
</script>

<style lang="less">
@import url("../assets/less/style.less");

#website-container {
    max-width: @container-width;
    margin: 0 auto;
    padding-top: @64px;

    #content-container {
      max-width: @content-width;
      margin: 0 auto;
      line-height: 23px;

      &.post {
        max-width: @post-width;
      }
    }
}

@media only screen and (max-width: @container-width) {
    #website-container {
        .text-size-small;
        padding: @32px;

        img {
            width: 100%;
            height: auto;
        }

        .hide {
            display: none;
        }
    }
}
</style>