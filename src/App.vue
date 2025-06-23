<template>
  <div class="min-h-screen flex flex-col">
    <HeaderComponent class="py-4" />
    <CoverGenerator class="flex-1" />
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
