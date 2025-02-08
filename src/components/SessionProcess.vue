<template>
    <div class="session-process">
        <div class="title">Tiến độ học</div>
        <div class="days-remaining">
            <span>Số ngày còn lại </span>
            <div class="count">
                {{ data.remaining_duration }} /{{ data.duration }} ngày
            </div>
        </div>
        <hr />
        <div class="earned-cups">
            <span>Số cúp đat được</span>
            <div class="count"> <img :src="trophy" alt="Cup">{{ data.missions.earned_cups }} /{{
                data.missions.total_cups }} </div>
        </div>
        <hr />
        <div class="progress-container">
            <span>Số Unit đạt {{ data.missions.actual_completed_units }} cúp/ Tổng số Unit:</span>
            <div class="progress">
                <div class="progress-fill actual" :style="{ width: actualProgress + '%' }"></div>
                <div class="progress-fill expected" :style="{ width: expectedProgress + '%' }"></div>
            </div>
            <div class="progress-bar">
                <span class="progress-label">
                    <div class="dot actual"></div>Thực tế: {{ data.missions.actual_completed_units }}/{{
                        data.missions.total_units }} Units
                </span>
                <span class="progress-label">
                    <div class="dot expected"></div>Kế hoạch: {{ data.missions.expected_completed_units }}/{{
                        data.missions.total_units }} Units
                </span>
            </div>

            <p class="status-label">{{ statusMessage }}</p>
        </div>
    </div>
</template>
<script setup>
import { computed } from 'vue';
import data from '@/server/api/data';
import trophy from '@/assets/images/trophy.svg';

const actualProgress = computed(() => {
    return (
        (data.missions.actual_completed_units / data.missions.total_units) *
        100
    ).toFixed(2);
});
const expectedProgress = computed(() => {
    return (
        (data.missions.expected_completed_units / data.missions.total_units) *
        100
    ).toFixed(2);
});

const statusMessage = computed(() => {
    if (
        data.missions.actual_completed_units >
        data.missions.expected_completed_units
    ) {
        return "Bạn đang học nhanh hơn kế hoạch";
    } else if (
        data.missions.actual_completed_units ===
        data.missions.expected_completed_units
    ) {
        return "Bạn đang học đúng với kế hoạch";
    } else {
        return "Bạn đang học chậm hơn kế hoạch";
    }
});

</script>
<style scoped>
.session-process {
    background-color: #fff;
    border-radius: 10px;
    padding: 10px;
    height: fit-content;
}

.title {
    font-weight: bold;
    font-size: 16px;
    margin-bottom: 10px;
}

.days-remaining,
.earned-cups {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.days-remaining span,
.earned-cups span,
.progress-container span {
    font-weight: 520;
    font-size: 14px;
    margin: 10px;
}

.count {
    color: #9ca4af;
    display: flex;
    align-items: center;
    gap: 5px;
    font-size: 14px;
}

hr {
    border: 0.1px solid #e5e5e5;
    margin: 0px;
}

.progress-bar {
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
}

.progress-label {
    font-size: 14px;
    margin-top: 15px;
    color: #9ca4af;
}

.progress {
    width: 100%;
    height: 15px;
    background-color: #e5e5e5;
    border-radius: 8px;
    overflow: hidden;
    position: relative;
    margin-top: 15px;
}

.progress-fill {
    height: 100%;
    top: 0;
    left: 0;
    background-color: #e5e5e5;
}

.progress-fill.actual {
    background-color: #30c55b;
    z-index: 2;
    position: relative;
    border-radius: 8px;
}

.progress-fill.expected {
    background-color: #0c70e6;
    z-index: 1;
    position: absolute;
    border-radius: 8px;
}

.status-label {
    font-size: 14px;
    color: #9ca4af;
    font-weight: bold;
}

.dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    display: inline-block;
    margin-right: 10px;
}

.dot.actual {
    background-color: #30c55b;
}

.dot.expected {
    background-color: #0c70e6;
}
</style>
