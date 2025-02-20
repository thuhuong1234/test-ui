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
            <AppMain />
          </div>
          <div class="dashboard-left">
            <SessionProcess />
            <label class="name">Nguyễn Thị Thu Hường</label>
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
  height: 100vh;
  background: linear-gradient(#007ff4, #a2d6ec);
}

.content {
  margin: 10px 40px;
}

.title {
  color: white;
  font-family: "Arial";
  font-size: 15px;
}

.level-name {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-size: 1.5pc;
  font-weight: 500;
}

.dashboard {
  display: flex;
  gap: 10px;
  margin-top: 10px;
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

.dashboard-left {
  display: flex;
  flex-direction: column;
  width: 20%;
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
  height: auto;
  max-width: 10100%;
}

hr {
  border: 0.1px solid #1790ff;
}
</style>
