<template>
  <div class="main-dashboard">
    <AppHeader />
    <main></main>
    <AppFooter />
  </div>
</template>
<script setup>
import AppHeader from "./components/AppHeader.vue";
</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.main-dashboard {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(#007ff4, #a2d6ec);
}
</style>
