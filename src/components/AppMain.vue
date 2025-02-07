<template>
    <div v-for="(sessions, month) in groupedSessions" :key="month">
        <div class="sessions">
            <div v-for="session in sessions" :key="session.overall_index" :class="['session', handleStatus(session)]">
                <div :class="['session-card', handleStatus(session)]">
                    <div class="session-header">
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

                    <div class="unit">
                        <ul :class="['unit-title', handleStatus(session)]">
                            <li>{{ getUnitTitle(session) }}</li>
                        </ul>
                        <div :class="['unit-status', handleStatus(session)]">{{ getUnitLabel(session) }}</div>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script setup>
import { ref, computed } from "vue";
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
    preCompleted: { value: 'pre-completed', icon: completedIcon, label: `Đã hoàn thành trước khi khởi tạo Study Plan` },
};

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
    const options = { weekday: 'long', month: 'short', day: 'numeric' };
    return new Date(date).toLocaleDateString('vi-VN', options);
}
const getUnitTitle = (session) => {
    const unitId = session.unit_ids[0];
    const unit = data.units.find((unit) => unit.unit_id === unitId);
    return unit ? unit.unit_title : "Unknown Unit";
};
const getUnitLabel = (session) => {
    if (session.completion_date) {
        if (handleStatus(session) === 'completed') {
            return `${sessionStatus.completed.label}: ${formatDate(session.completion_date)}`
        } if (handleStatus(session) === 'lated') {
            return sessionStatus.lated.label
        }
    }
    if (session.completion_date === null) {
        if (handleStatus(session) === 'completed') {
            return sessionStatus.preCompleted.label
        } if (handleStatus(session) === 'lated') {
            return sessionStatus.lated.label
        }

    }
}

const groupedSessions = computed(() => {
    let sessionsWithDate = sessions.value
        .filter(session => session.date)
        .sort((a, b) => new Date(a.date) - new Date(b.date));

    let grouped = {};
    sessionsWithDate.forEach(session => {
        let month = new Date(session.date).toLocaleString("vi-VN", { month: "long", year: "numeric" });
        if (!grouped[month]) grouped[month] = [];
        grouped[month].push(session);
    });

    let sessionsWithoutDate = sessions.value
        .filter(session => !session.date)
        .sort((a, b) => a.sessionNumber - b.sessionNumber);

    if (sessionsWithoutDate.length) {
        grouped["Not Date"] = sessionsWithoutDate;
    }

    return grouped;
});
</script>

<style scoped>
.sessions {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    flex-wrap: wrap;
    gap: 10px;
}

.session-header {
    display: flex;
    gap: 10px;
    padding: 10px;
    align-items: center;
    border-radius: 8px;
    color: white;
    transition: background-color 0.3s ease;
    justify-content: space-between;
}

.session-card {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding: 10px;
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
    align-items: center;
    display: flex;
    justify-content: center;
    gap: 5px;
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

.unit {
    color: black;
}

.unit-title {
    color: #616b75;
}

.unit-title.completed li::marker {
    font-size: 20px;
    color: #06ae32;
}

.unit-title.lated li::marker {
    font-size: 20px;
    color: #f89c1b;
}

.unit-title.future li::marker {
    font-size: 20px;
    color: #0c70e6;
}

.unit-title.todayCompleted li::marker {
    font-size: 20px;
    color: #06ae32;
}

.unit-title.todayIncomplete li::marker {
    font-size: 20px;
    color: #0c70e6;
}

.unit-title.completed li:hover {
    text-decoration: underline;
    color: #06ae32;
}

.unit-title.lated li:hover {
    text-decoration: underline;
    color: #f89c1b;
}

.unit-title.future li:hover {
    text-decoration: underline;
    color: #0c70e6;
}

.unit-title.todayCompleted li:hover {
    text-decoration: underline;
    color: #06ae32;
}

.unit-title.todayIncomplete li:hover {
    color: #0c70e6;
    text-decoration: underline;
}

.unit-status {
    font-weight: 500;
    margin-left: 15px;
}

.unit-status.completed {
    color: #06ae32;
}

.unit-status.lated {
    color: #f89c1b;
}
</style>