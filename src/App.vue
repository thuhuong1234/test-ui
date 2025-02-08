<template>
  <div class="main-dashboard">
    <AppHeader />
    <hr />
    <main class="content">
      <div class="title">
        <label class="level-name">
          Chặng: {{ data.level_name }}
        </label>
        <div class="dashboard">
          <div class="sessions-container">
            <div class="sessions-header">
              <button class="sessions-btn">Hôm nay</button>
              <span class="sessions-title">Tổng quan</span>
            </div>
            <hr class="hr" />
            <AppMain />
          </div>
          <div class="dashboard-left">
            <SessionProcess />
            <div class="name">Nguyễn Thị Thu Hường</div>
            <div class="img-cover"><img :src="cover" alt=""></div>
          </div>
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
import cover from "@/assets/images/cover.svg";</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.main-dashboard {
  width: 100%;
  height: fit-content;
  background: linear-gradient(#007ff4, #a2d6ec);
}

.content {
  padding: 0 40px;
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

.sessions-container {
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

.dashboard-left {
  display: flex;
  flex-direction: column;
  width: 20%;
  padding: 10px;
  margin-left: 40px;
  color: black;
  gap: 10px;
}

.name {
  font-weight: bold;
  font-size: 16px;
  padding: 20px;
  border-radius: 10px;
  background-color: #55A8F6;
  color: #c6f9ff;
  text-align: center;
}

.img-cover {
  display: flex;
  justify-content: center;
}

hr {
  border: 0.1px solid #e5e5e5;
}
</style>
