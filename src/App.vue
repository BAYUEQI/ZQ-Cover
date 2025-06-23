<template>
  <div class="min-h-screen flex flex-col bg-gradient-to-br from-gray-50 to-gray-100">
    <HeaderComponent />
    <CoverGenerator class="flex-1 py-6 px-4 sm:px-6 lg:px-8" />
  </div>
</template>

<script>
import HeaderComponent from './components/HeaderComponent.vue';
import CoverGenerator from './components/CoverGenerator.vue';

export default {
  components: {
    HeaderComponent,
    CoverGenerator
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

body {
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
}

/* 添加平滑滚动 */
html {
  scroll-behavior: smooth;
}

/* 全局过渡效果 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
