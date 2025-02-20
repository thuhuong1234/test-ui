<template>
    <div class="container">
        <div class="sessions-header">
            <button class=" sessions-btn" @click="scrollToTodaySession">Hôm nay</button>
            <span class="sessions-title">Tổng quan</span>
        </div>
        <div class="line"></div>
        <div class="sessions">
            <div v-for="(sessions, month) in groupedSessions" :key="month">
                <div class="header-line">
                    <div class="line"></div>
                    <div class="month">
                        {{ month }}
                    </div>
                    <div class="line"></div>
                </div>
                <div class="session-list">
                    <div v-for="session in sessions" :key="session.overall_index"
                        :class="['session', handleStatus(session)]">
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
const getMonthYear = (date) => {
    let dateObj = new Date(date);
    return `Tháng ${dateObj.getMonth() + 1}, ${dateObj.getFullYear()}`;
};

const sortSessions = (sessions) => {
    return [...sessions].sort((a, b) => a.overall_index - b.overall_index);
};

const groupSessionsByMonth = (sessions) => {
    const grouped = {};
    let lastKnownMonth = null;
    let firstMonthFound = false;

    sessions.forEach(session => {
        if (session.date) {
            let monthYear = getMonthYear(session.date);

            if (!firstMonthFound) {
                lastKnownMonth = monthYear;
                firstMonthFound = true;
            }

            grouped[monthYear] = grouped[monthYear] || [];
            grouped[monthYear].push(session);
            lastKnownMonth = monthYear;
        } else {
            if (lastKnownMonth) {
                grouped[lastKnownMonth].push(session);
            } else {
                for (let s of sessions) {
                    if (s.date) {
                        let firstMonth = getMonthYear(s.date);
                        grouped[firstMonth] = grouped[firstMonth] || [];
                        grouped[firstMonth].push(session);
                        break;
                    }
                }
            }
        }
    });

    return grouped;
};

const groupedSessions = computed(() => {
    if (!sessions.value || !Array.isArray(sessions.value)) return {};

    const sortedSessions = sortSessions(sessions.value);
    return groupSessionsByMonth(sortedSessions);
});

const scrollToTodaySession = () => {
    let todaySessionCard = null;
    if (document.querySelector('.session-card.todayCompleted')) {
        todaySessionCard = document.querySelector('.session-card.todayCompleted');
    } else {
        todaySessionCard = document.querySelector('.session-card.todayIncomplete');
    }
    todaySessionCard?.scrollIntoView({ behavior: 'smooth', block: 'center' });
};

</script>

<style scoped>
.sessions {
    flex: 1;
    overflow-y: auto;
    max-height: 80vh;
    padding: 10px;
    scrollbar-width: thin;
    scrollbar-color: #888 #e5e5e5;
}

.session-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
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
    border: 2px solid #0c70e6;
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

.header-line {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 5px;
}

.line {
    border: 0.1px solid #e5e5e5;
    width: 100%;
}

.month {
    display: flex;
    justify-content: center;
    padding: 10px;
    width: 40%;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 15px;
    font-weight: 500;
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
    cursor: pointer;
}

.sessions-title {
    padding: 8px;
    font-weight: bold;
}
</style>