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
  gap: 20px;
  margin-top: 10px;
}

.sessions-container {
  background-color: #fff;
  border-radius: 5px;
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
  width: 25%;
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
  max-width: 100%;
}

hr {
  border: 0.1px solid #1790ff;
  margin: 0;
}

@media(max-width: 424px) {
  .session-list {
    grid-template-columns: repeat(1, 250px);
    align-items: center;
  }

  .dashboard-left {
    order: -1;
    width: 100%;
  }

  .dashboard {
    width: 100%;
    flex-direction: column;
  }

  .sessions-container {
    width: 100%;
    padding: 0;
  }

  .main-dashboard {
    height: 100%;
  }

  .name,
  .img-cover {
    display: none;
  }

  .content {
    margin: 5px;
  }

  .sessions {
    padding: 0;
  }
}

@media(min-width:425px) and (max-width: 767px) {
  .session-list {
    grid-template-columns: repeat(2, 1fr);
  }

  .dashboard-left {
    order: -1;
    width: 100%;
  }

  .dashboard {
    width: 100%;
    flex-direction: column;
  }

  .sessions-container {
    width: 100%;
    padding: 0;
  }

  .main-dashboard {
    height: 100%;
  }

  .name,
  .img-cover {
    display: none;
  }
}

@media (min-width: 768px) and(max-width: 1024px) {
  .session-list {
    grid-template-columns: repeat(1, 1fr);

  }

}

@media (min-width: 1025px) and (max-width: 1280px) {
  .session-list {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1281px) and (max-width: 1440px) {
  .session-list {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>