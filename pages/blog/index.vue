<template>
  <div>
    <h1>Blog</h1>
    <ul class="posts">
      <li class="post">
        <h4 class="title"><a href="/blog/mistakes-using-javascript-promises">Mistakes we make using JavaScript Promises</a></h4>
        <p class="date">Wednesday, May 29, 2019</p>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'

@Component({
  head() {
    return {
      title: 'Betamark - Blog'
    }
  }
})
export default class Blog extends Vue {
  
}
</script>

<style lang="less" scoped>
@import url("../../assets/less/style.less");

.posts {
  padding: 0px;
  margin: 0px;
  list-style-type: none;

  .post {
    margin-bottom: @8px;
    .title {
      margin-bottom: 0px;
      margin-top: 0px;
    }
    .date {
      .text-size-tiny;
      margin-top: 0px;
      margin-bottom: 0px;
    }
  }
}
</style>