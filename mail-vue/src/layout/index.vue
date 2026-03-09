<template>
  <el-container class="layout">
    <el-aside
        class="aside"
        :class="uiStore.asideShow ? 'aside-show' : 'el-aside-hide'">
      <Aside />
    </el-aside>
    <div
        :class="(uiStore.asideShow && isMobile)? 'overlay-show':'overlay-hide'"
        @click="uiStore.asideShow = false"
    ></div>
    <el-container class="main-container">
      <el-main>
        <el-header>
            <Header />
        </el-header>
        <Main />
      </el-main>
    </el-container>
  </el-container>
  <writer ref="writerRef" />
</template>

<script setup>
import Aside from '@/layout/aside/index.vue'
import Header from '@/layout/header/index.vue'
import Main from '@/layout/main/index.vue'
import { ref, onMounted, onBeforeUnmount } from 'vue'
import {useUiStore} from "@/store/ui.js";
import writer from '@/layout/write/index.vue'

const uiStore = useUiStore();
const writerRef = ref({})
const isMobile = ref(window.innerWidth < 1025)
const handleResize = () => {
  isMobile.value = window.innerWidth < 1025
  uiStore.asideShow = window.innerWidth > 1024;
}

onMounted(() => {
  uiStore.writerRef = writerRef

  window.addEventListener('resize', handleResize)
  handleResize()
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize)
})
</script>

<style lang="scss" scoped>
.el-aside-hide {
  position: fixed;
  left: 0;
  height: 100%;
  z-index: 100;
  transform: translateX(-100%);
  transition: all var(--anime-motion-fast);
}

.aside-show {
  -webkit-box-shadow: var(--anime-shadow-elevated);
  box-shadow: var(--anime-shadow-elevated);
  transform: translateX(0);
  transition: all var(--anime-motion-fast);
  z-index: 101;
  @media (max-width: 1025px) {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 101;
    height: 100%;
    background: var(--anime-aside-bg);
  }
}

.el-aside {
  width: auto;
  transition: all var(--anime-motion-fast);
}

.layout {
  height: 100%;
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  overflow: hidden;
  background: var(--anime-bg-gradient);
}

.main-container {
  min-height: 100%;
  background: var(--anime-main-surface);
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  backdrop-filter: blur(8px);
}

.el-main {
  padding: 0;
}

.el-header {
  background: var(--anime-header-bg);
  border-bottom: solid 1px var(--anime-border-soft);
  padding: 0;
  backdrop-filter: blur(8px);
}

.overlay-show {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(13, 20, 36, 0.42);
  backdrop-filter: blur(4px);
  z-index: 99;
  transition: all var(--anime-motion-normal);
}

.overlay-hide {
  display: flex;
  pointer-events: none;
  opacity: 0;
}
</style>
