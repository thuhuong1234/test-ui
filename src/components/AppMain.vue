<template>
    <div class="sessions">
        <div v-for="session in sessions" :key="session.overall_index" :class="['session', handleStatus(session)]">
            <div :class="['session-card', handleStatus(session)]" :v-if="session.overall_index <= 10">
                <div :class="['session-number', handleStatus(session)]">
                    Buổi {{ session.overall_index }}
                    <img :src="sessionStatus[handleStatus(session)].icon" alt="Status Icon">
                </div>
                <div class="session-date" v-if="handleStatus(session) !== 'completed'">
                    {{ formatDate(session.date) }}
                </div>
                <div class="session-ward">
                    <img :src="trophyIcon" alt="Trophy Icon">
                    <div class="ward-text">0/9</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from "vue";
import data from '@/server/api/data';
import completedIcon from '@/assets/images/done.svg'
import futureIcon from '@/assets/images/process.svg'
import latedIcon from '@/assets/images/warn.svg';
import trophyIcon from '@/assets/images/trophy.svg';
const sessions = ref([]);
sessions.value = data.sessions;

const sessionStatus = {
    completed: { value: 'completed', icon: completedIcon, label: `Đã hoàn thành ` },
    lated: { value: 'lated', icon: latedIcon, label: `Bạn chưa hoàn thành buổi học này` },
    future: { value: 'future', icon: futureIcon, label: `` },
    todayCompleted: { value: 'today-completed', icon: completedIcon, label: `` },
    todayIncomplete: { value: 'today-incomplete', icon: latedIcon, label: `` },
}

const today = new Date();
const handleStatus = (session) => {
    const date = new Date(session.date);
    date.setHours(0, 0, 0, 0);
    today.setHours(0, 0, 0, 0);

    if (date < today) {
        return session.completed ? 'completed' : 'lated';
    } else if (date.getTime() === today.getTime()) {
        return session.completed ? 'todayCompleted' : 'todayIncomplete';
    } else if (date > today) {
        return 'future';
    }

}
const formatDate = (date) => {
    const options = { weekday: 'short', month: 'short', day: 'numeric' };
    return new Date(date).toLocaleDateString('vi-VN', options);
}
</script>

<style scoped>
.sessions {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
}

.session-card {
    display: flex;
    gap: 10px;
    padding: 10px;
    align-items: center;
    border-radius: 8px;
    color: white;
    transition: background-color 0.3s ease;
}

.session-card.completed {
    background-color: #f0fdf4;
}

.session-card.lated {
    background-color: #fefbea;
}

.session-card.future {
    background-color: #f9fafc;
}

.session-card.todayCompleted {
    background-color: #e6ffe6;
    border-color: #06ae32;
}

.session-card.todayIncomplete {
    background-color: #e6f2ff;
    border: 1px solid #0c70e6;
}

.session-number {
    color: white;
    border-radius: 10px;
    padding: 5px;
    height: 20px;
    width: 80px;
    text-align: center;

}

.session-number.completed {
    background-color: #06ae32;
}

.session-number.lated {
    background-color: #f89c1b;
}

.session-number.future {
    background-color: #9ca4af;
}

.session-number.todayCompleted {
    background-color: #06ae32;
    border-color: #06ae32;
}

.session-number.todayIncomplete {
    background-color: #0c70e6;
}

.session-date {
    color: black;
    font-weight: 600;
}

.session-ward {
    color: #9ca4af;
    font-weight: 600;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 5px;
}
</style>