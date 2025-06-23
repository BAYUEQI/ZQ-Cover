<template>
  <div class="min-h-screen flex flex-col bg-gradient-to-br from-gray-50 to-gray-100">
    <HeaderComponent class="py-4 shadow-md" />
    <CoverGenerator class="flex-1 transition-all duration-300 ease-in-out" />
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
:root {
  --vue-app-font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
}

body {
  font-family: var(--vue-app-font-family);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* 添加全局过渡效果 */
.transition-all {
  transition-property: all;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 300ms;
}
</style>
