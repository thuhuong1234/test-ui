<template>
  <div class="main-dashboard">
    <AppHeader />
    <main class="content">
      <div class="title">
        <label class="level-name">
          Chặng: {{ data.level_name }}
        </label>
        <div class="dashboard">
          <div class="sessions">
            <div class="sessions-header">
              <div class="sessions-btn">Hôm nay</div>
              <div class="sessions-title">Tổng quan</div>
            </div>
            <hr class="hr" />
            <div class="month">
              Tháng 10, 2024

            </div>
            <AppMain />
          </div>
          <SessionProcess />
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
import SessionProcess from "./components/SessionProcess.vue";
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
  display: flex;
  gap: 10px;
}

.sessions {
  background-color: #fff;
  border-radius: 5px;
  padding: 10px;
  color: black;
  width: 75%;
}

.level-name {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-size: large;
}

.sessions-header {
  display: flex;
  gap: 10px;
  padding-bottom: 10px;
}

.sessions-btn {
  border: 1px solid #cbcbcb;
  border-radius: 7px;
  padding: 8px;
  font-weight: bold;
  font-size: 13px;
}

.sessions-title {
  padding: 8px;
  font-weight: bold;
}

.month {
  display: flex;
  justify-content: center;
}
</style>
