<template>
  <div class="vcomment" v-if="data.comments !== false" :class="{'hasSidebarAndPc':hasSidebar}">
    <div id="vcomments"></div>
  </div>
</template>

<script>
import { isActive, hashRE, groupHeaders } from "../../util";
export default {
  name: "Valine",
  props: {
    hasSidebar: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    data() {
      return this.$page.frontmatter;
    }
  },
  mounted() {
    console.log(this.hasSidebar);
    this.createValine();
  },

  methods: {
    createValine() {
      const valineConfig = this.$themeConfig.valineConfig;
      if (!valineConfig) return;
      const AV = require("leancloud-storage");
      const Valine = require("valine");
      if (typeof window !== "undefined") {
        this.window = window;
        window.AV = AV;
      } else {
        return;
      }
      const valine = new Valine({
        el: "#vcomments",
        appId: valineConfig.appId,
        appKey: valineConfig.appKey,
        placeholder: valineConfig.placeholder || "just go go",
        notify: valineConfig.notify || false,
        verify: valineConfig.verify || false,
        avatar: valineConfig.avatar || "monsterid",
        visitor: valineConfig.visitor || true, // 阅读量统计
        recordIP: valineConfig.recordIP || false,
        path: window.location.pathname
      });
      this.valineRefresh = false;
    }
  },
  watch: {
    $route(to, from) {
      if (to.path !== from.path) {
        setTimeout(() => {
          //重新刷新valine
          this.createValine();
        }, 300);
      }
    }
  }
};
</script>

<style lang="stylus" scoped>
@media (max-width: $MQMobile) {
  .hasSidebarAndPc {
    margin-left: 0rem !important;
  }
}

.hasSidebarAndPc {
  margin-left: 20rem;
}

#vcomments {
  max-width: 740px;
  padding: 10px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>