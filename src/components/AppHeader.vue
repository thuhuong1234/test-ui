<template>
    <header>
        <div class="app-header">
            <div class="app-menu">
                <img class="menu-btn" :src="menuIcon" alt="Menu">
                <div class="app-logo">
                    <img class="logo" :src="logoIcon" alt="Logo">
                </div>
            </div>

            <div class="app-account">
                <img class="account" width="40px" height="40px" :src="defaultAvatar" alt="Account">
            </div>
        </div>
    </header>
</template>

<script setup>
import menuIcon from '@/assets/images/menu.svg';
import logoIcon from '@/assets/images/logo.svg';
import defaultAvatar from '@/assets/images/default-avatar.jpeg';
</script>

<style scoped>
.app-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 7px 40px;
}

.app-menu {
    display: flex;
    gap: 50px;
}

.app-account {
    border: 1px solid white;
    border-radius: 50%;
    width: 40px;
    height: 40px;
}

.account {
    border-radius: 50%;
}
</style>
