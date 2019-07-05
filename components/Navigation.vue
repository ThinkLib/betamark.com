<template>
  <div id="navigation">
    <div class="header">
      <div class="logo">
          <span class="icon">💾</span> Shlomi Nissan<span class="hide">,</span>
          <span class="subtitle hide">Software Engineer</span>
      </div>
      <div class="social-media">
        <div class="sm-icon github" @click="goto('https://github.com/shlomnissan')">
        </div>
        <div class="sm-icon twitter" @click="goto('https://twitter.com/shlominissan')">
        </div>
      </div>
    </div>
    <div class="navigation">
      <ul>
        <li :class="{'active': routeName == 'index'}">
          <a href="/">About</a>
        </li>
        <li :class="{'active': routeName == 'work'}">
          <a href="/work">Work</a>
        </li>
        <li :class="{'active': routeName == 'blog' || routeName == 'blog-post'}">
          <a href="/blog">Blog</a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator"

@Component
export default class Navigation extends Vue {
  get routeName() {
    return this.$route.name;
  }

  goto(url) {
    window.open(url, '_blank');
  }

  
}
</script>

<style lang="less" scoped>
@import url("../assets/less/style.less");

#navigation {
    .header {
        display: flex;
        border-bottom: 1px solid @color-gray-light;
        .logo {
            font-weight: 600;
            font-size: @text-size-h4;
            margin-bottom: @16px;

            .icon {
              padding-right: @8px;
            }
            .subtitle {
                font-weight: 400;
            }
        }

        .social-media {
          flex: 1;
          justify-content: flex-end;
          align-items: center;
          display: flex;
          text-align: right;

          .sm-icon {
            cursor: pointer;
            margin-bottom: @16px;
            background: @color-gray-dark;
            mask-size: cover;

            &:hover {
              background: @color-text;
            }
          }

          .github {
            width: 20px;
            height: 20px;
            margin-right: @16px;
            mask: url(../static/icon-github.svg);
          }

          .twitter {
            width: 22px;
            height: 19px;
            mask: url(../static/icon-twitter.svg);
          }
        }
    }
    .navigation {
        flex: 1;
        ul {
            padding: 0px;
            margin: 0px;
            margin-bottom: @48px;
            list-style: none;

            li {
                display: inline-block;
                padding-right: @24px;

                a {
                    .color-gray-dark;
                    font-size: @text-size-small;
                    font-weight: 500;
                    display: inline-block;
                    padding-top: @16px;
                    text-decoration: none;
                }

                &.active {
                    a {
                        color: @color-primary;
                        border-top: 1px solid @color-primary;
                    }
                }
            }
        }
    }
}
</style>