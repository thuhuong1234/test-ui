<template>
  <div class="main-dashboard">
    <AppHeader />
    <main class="content">
      <div class="title">
        <div class="level-name">
          Chặng: {{ data.level_name }}
        </div>
        <AppMain />
      </div>
    </main>
    <AppFooter />
  </div>
</template>
<script setup>
import AppHeader from "@/components/AppHeader.vue";
import AppMain from "@/components/AppMain.vue";
import data from "@/server/api/data";
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

.content {
  padding: 0 40px;
  border-top: 1px solid rgb(255, 255, 255);
}

.title {
  padding: 20px 0;
  color: white;
  font-family: "Arial";
  font-size: 15px;
}

.level-name {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-size: 1.5pc;
  font-weight: 500;
  margin-left: 40px;
}
</style>
