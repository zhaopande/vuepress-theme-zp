<template>
  <div class="article-Info">
    <i
      class="iconfont zpicon-person"
      v-if="
        articleInfo.frontmatter.author || $themeConfig.author || $site.title
      "
    >
      <span class="ml5">{{
        articleInfo.frontmatter.author || $themeConfig.author || $site.title
      }}</span>
    </i>
    <i class="iconfont zpicon-date" v-if="articleInfo.frontmatter.date">
      <span class="ml5">{{
        new Date(articleInfo.frontmatter.date).toLocaleDateString()
      }}</span>
    </i>
    <!-- <i class="iconfont zpicon-date" v-if="articleInfo.frontmatter.date"></i> -->
    <!-- <span>{{ new Date(articleInfo.frontmatter.date).toLocaleDateString() }}</span> -->
    <i class="iconfont zpicon-book1 tags" v-if="articleInfo.frontmatter.tag">
      <span class="tag-item" @click="goTags(articleInfo.frontmatter.tag)">{{
        articleInfo.frontmatter.tag
      }}</span>
    </i>
    <AccessCount :path="isList ? '/' : articleInfo.path" />
  </div>
</template>
<!-- <AccessNumber :idVal="articleInfo.path" :numStyle="numStyle"></AccessNumber> -->

<script>
import AccessCount from "./Valine/AccessCount";
import { goTags } from "@theme/util";
export default {
  name: "ArticleInfo",
  components: { AccessCount },
  props: {
    articleInfo: { type: Object, default: {} },
    currentTag: { type: String, default: "" },
    isList: { type: Boolean, default: false }, //是否
  },
  mounted() {},
  data() {
    return {
      numStyle: {
        fontSize: ".9rem",
        fontWeight: "normal",
        color: "#999",
      },
      goTags: goTags,
    };
  },

  methods: {},
};
</script>

<style lang="stylus" scoped>
.article-Info {
  display: flex;
  justify-content: start;

  .iconfont {
    display: inline-block;
    line-height: 1.5rem;

    &:not(:last-child) {
      margin-right: 0.8rem;
    }

    span {
      // margin-left: 0.5rem;
    }
  }

  .tags {
    .tag-item {
      cursor: pointer;

      &:first-child {
        margin-left: 0.5rem;
      }

      &.active {
        color: $accentColor;
      }

      &:hover {
        color: $accentColor;
        transform: scale(1.15);
      }
    }
  }
}

@media (max-width: $MQMobile) {
  .tags {
    display: block;
    margin-left: 0 !important;
  }

  .article-Info {
    flex-wrap: wrap;
  }
}
</style>
