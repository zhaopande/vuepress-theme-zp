<template>
  <div
    class="theme-container"
    :class="pageClasses,themeClass"
    @touchstart="onTouchStart"
    @touchend="onTouchEnd"
    ref="mostTop"
  >
    <Navbar v-if="shouldShowNavbar" @toggle-sidebar="toggleSidebar" @themeColor="colorValue" />

    <div class="sidebar-mask" @click="toggleSidebar(false)"></div>

    <Sidebar :items="sidebarItems" @toggle-sidebar="toggleSidebar">
      <slot name="sidebar-top" slot="top" />
      <slot name="sidebar-bottom" slot="bottom" />
    </Sidebar>
    <slot></slot>
    <Valine v-if="isShowComment" :hasSidebar="shouldShowSidebar" />
    <BackToTop />
  </div>
</template>

<script>
import Navbar from "@theme/components/Navbar.vue";
import Sidebar from "@theme/components/Sidebar.vue";
import BackToTop from "@theme/components/BackToTop.vue";
import Valine from "@theme/components/Valine";
import { resolveSidebarItems } from "../util";

export default {
  components: {
    Sidebar,
    Navbar,
    BackToTop,
    Valine
  },

  data() {
    return {
      isSidebarOpen: false,
      themeClass: []
    };
  },
  props: {
    sidebarShow: {
      type: Boolean,
      default: true
    }
  },

  computed: {
    isShowComment() {
      return (
        (!this.$page.frontmatter.home &&
          !this.$page.frontmatter.isTag &&
          !this.$page.frontmatter.isTimeLine &&
          !this.$page.path.includes("categories") &&
          (this.$page.frontmatter.comments !== undefined
            ? this.$page.frontmatter.comments
            : true)) ||
        false
      );
    },
    shouldShowNavbar() {
      const { themeConfig } = this.$site;
      const { frontmatter } = this.$page;
      if (frontmatter.navbar === false || themeConfig.navbar === false) {
        return false;
      }
      return (
        this.$title ||
        themeConfig.logo ||
        themeConfig.repo ||
        themeConfig.nav ||
        this.$themeLocaleConfig.nav
      );
    },

    shouldShowSidebar() {
      const { frontmatter } = this.$page;
      return (
        !frontmatter.home &&
        frontmatter.sidebar !== false &&
        this.sidebarItems.length &&
        this.sidebarShow
      );
    },

    sidebarItems() {
      return resolveSidebarItems(
        this.$page,
        this.$page.regularPath,
        this.$site,
        this.$localePath
      );
    },

    pageClasses() {
      const userPageClass = this.$page.frontmatter.pageClass;
      return [
        {
          "no-navbar": !this.shouldShowNavbar,
          "sidebar-open": this.isSidebarOpen,
          "no-sidebar": !this.shouldShowSidebar
        },
        userPageClass
      ];
    }
  },

  mounted() {
    console.log(this.shouldShowSidebar);
    this.$router.afterEach(() => {
      this.isSidebarOpen = false;
    });
  },

  methods: {
    toggleSidebar(to) {
      console.log(to);
      this.isSidebarOpen = typeof to === "boolean" ? to : !this.isSidebarOpen;
    },

    // side swipe
    onTouchStart(e) {
      this.touchStart = {
        x: e.changedTouches[0].clientX,
        y: e.changedTouches[0].clientY
      };
    },

    onTouchEnd(e) {
      const dx = e.changedTouches[0].clientX - this.touchStart.x;
      const dy = e.changedTouches[0].clientY - this.touchStart.y;
      if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 40) {
        if (dx > 0 && this.touchStart.x <= 80) {
          this.toggleSidebar(true);
        } else {
          this.toggleSidebar(false);
        }
      }
    },
    colorValue: function(val) {
      console.log(val);
      // this.themeClass = {
      //   "$accentColor": val
      // };
      this.themeClass = [
        {
          "black-theme": true
        }
      ];
      console.log(this.$refs.mostTop.classList);

      document.body.classList.add("black-theme");
    }
  }
};
</script>

<style src="prismjs/themes/prism-tomorrow.css"></style>
<style src="../styles/index.styl" lang="stylus"></style>
<style src="../styles/icon.styl" lang="stylus"></style>


