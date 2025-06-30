<template>
  <div id="app">
    <EChartsExample />
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import EChartsExample from './components/EChartsExample.vue';

export default defineComponent({
  name: 'App',
  components: {
    EChartsExample,
  },
});
</script>

<style>
/* 可以在这里添加全局样式 */
</style>
