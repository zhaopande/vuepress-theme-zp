<template lang="">
    <div class="home-Bg-Box">
        <div :style="bgImageStyle"></div>
    </div>
</template>
<script>
export default {
  name: "HomeBg",
  props: {},
  data() {
    return {};
  },
  computed: {
    bgImageStyle() {
      const initBgImageStyle = {
        textAlign: "center",
        overflow: "hidden",
        width: "100%",
        height: "100%",
        background: `
          url(${this.$withBase(
            this.$site.themeConfig.homeBg
          )}) center/cover no-repeat
        `,
      };

      return initBgImageStyle;
    },
  },
  mounted() {},
  methods: {},
};
</script>
<style lang="stylus">
.home-Bg-Box {
  height: 400px;
  text-align: center;
  overflow: hidden;
  margin-top: 57px;
}
</style>