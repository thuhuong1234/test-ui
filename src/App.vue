<template>
  <div class="main-dashboard">
    <AppHeader />
    <main class="content">
      <div class="title">
        <label class="level-name">
          Chặng: {{ data.level_name }}
        </label>
        <div class="dashboard">
          <div class="dashboard-header">
            <div class="dashboard-btn">Hôm nay</div>
            <div class="dashboard-title">Tổng quan</div>
          </div>
          <hr class="hr" />
          <div class="month">
            Tháng 10, 2024

          </div>
          <AppMain />
        </div>

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

.dashboard {
  background-color: #fff;
  border-radius: 5px;
  padding: 10px;
  margin-left: 40px;
  color: black;
}

.level-name {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-size: large;
}

.dashboard-header {
  display: flex;
  gap: 10px;
  padding-bottom: 10px;
}

.dashboard-btn {
  border: 1px solid #cbcbcb;
  border-radius: 7px;
  padding: 8px;
  font-weight: bold;
  font-size: 13px;
}

.dashboard-title {
  padding: 8px;
  font-weight: bold;
}

.month {
  display: flex;
  justify-content: center;
}
</style>
