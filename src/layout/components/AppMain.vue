<template>
  <section class="app-main">
    <transition name="fade-transform" mode="out-in">
      <!-- app-main 外部包了一层 keep-alive 主要是为了缓存 <router-view> 的，
      配合页面的 tabs-view 标签导航使用，如不需要可自行去除 -->
      <keep-alive :include="cachedViews">
        <!--
          在 router-view 上加上一个唯一的 key，
          来保证路由切换时都会重新渲染触发钩子了。
          解决不同的路由每次都渲染相同的组件
        -->
        <router-view :key="key" />
      </keep-alive>
    </transition>
  </section>
</template>

<script>
export default {
  name: 'AppMain',
  computed: {
    cachedViews() {
      return this.$store.state.tagsView.cachedViews
    },
    key() {
      // 只要保证 key 唯一性就可以了，保证不同页面的 key 不相同
      return this.$route.path
    }
  }
}
</script>

<style lang="scss" scoped>
.app-main {
  /* 50= navbar  50  */
  min-height: calc(100vh - 50px);
  width: 100%;
  position: relative;
  overflow: hidden;
}

.fixed-header+.app-main {
  padding-top: 50px;
  height: 100vh;
  overflow: auto;
}

.hasTagsView {
  .app-main {
    /* 84 = navbar + tags-view = 50 + 34 */
    min-height: calc(100vh - 84px);
  }

  .fixed-header+.app-main {
    padding-top: 84px;
  }
}
</style>

<style lang="scss">
// fix css style bug in open el-dialog
.el-popup-parent--hidden {
  .fixed-header {
    padding-right: 15px;
  }
}
</style>
