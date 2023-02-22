<script setup>
    defineProps({
        status: String,
        statusText: String
    })
</script>

<template>
    <div class="notification" :class="'notification_' + status">
        <span>{{ statusText }}</span>
    </div>
</template>

<style scoped lang="scss">
    .notification {
        position: fixed;
        bottom: 20px;
        right: 20px;
        border-radius: 20px;
        padding: 20px;
        color: white;

        &_success {
            background-color: green;
        }

        &_failure {
            background-color: red;
        }
    }
</style>