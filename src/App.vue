<template>
  <div id="app">
    <home></home>
  </div>
</template>

<script>
import home from '@/views/home.vue'
export default {
  name: 'app',
  components: {
    home
  }
}
</script>

<style lang="scss">
@import "@/assets/main.scss";

</style>
