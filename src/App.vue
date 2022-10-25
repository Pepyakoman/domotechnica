<template>
  <news-page />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import NewsPage from './components/News-Page.vue';

export default defineComponent({
  name: 'App',
  components: {
    NewsPage,
  },
});
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}
body {
  margin: 0;
}
</style>
