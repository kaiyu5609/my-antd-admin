<template>
  <div :class="classObj" class="app-wrapper">
    <div v-if="device==='mobile' && sidebar.opened" class="drawer-bg" @click="handleClickOutside"></div>
    <sidebar class="sidebar-container" />
    <div class="main-container">
      <div :class="{'fixed-header':fixedHeader}">
        <navbar />
        <tags-view v-if="needTagsView" />
      </div>
      <app-main :class="{'fixed':fixedHeader,'need-tags-view':needTagsView}" />
      <right-panel v-if="showSettings">
        <settings />
      </right-panel>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";

import {
  Sidebar,
  Navbar,
  AppMain,
  RightPanel,
  Settings,
  TagsView
} from "./components";

import ResizeMixin from "./mixin/ResizeHandler";

export default {
  name: "Layout",
  components: {
    Sidebar,
    Navbar,
    AppMain,
    RightPanel,
    Settings,
    TagsView
  },
  mixins: [ResizeMixin],
  computed: {
    ...mapGetters(["sidebar", "device"]),
    needTagsView() {
      return this.$store.state.settings.tagsView;
    },
    fixedHeader() {
      return this.$store.state.settings.fixedHeader;
    },
    showSettings() {
      return this.$store.state.settings.showSettings;
    },
    classObj() {
      return {
        hideSidebar: !this.sidebar.opened,
        openSidebar: this.sidebar.opened,
        withoutAnimation: this.sidebar.withoutAnimation,
        mobile: this.device === "mobile"
      };
    }
  },
  methods: {
    handleClickOutside() {
      this.$store.dispatch("app/closeSideBar", { withoutAnimation: false });
    }
  }
};
</script>

<style lang="scss" scoped>
</style>
