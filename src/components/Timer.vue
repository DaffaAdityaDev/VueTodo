<template>
    <div class="wrapper">
        <h1>Current Todo Running</h1>
        <p class="todo--title">{{ todo?.text }}</p>
        <p class="todo--timer">{{ formatTimer(time) }}</p>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';

defineProps({
  time: {
    type: Number,
    required: true,
  },
  todo : {
    type: Object,
    required: true,
  }
});

const formatTimer = (time) => {
    const hour = Math.floor(time / 3600);
    const minute = Math.floor((time % 3600) / 60);
    const second = Math.floor((time % 3600) % 60);
    return `${hour}:${minute}:${second}`;
};


</script>

<style scoped>

.wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-top: 1rem;
}

.todo--title {
    font: 500 2rem 'Roboto', sans-serif;
    color: white;
    text-align: center;
    margin-bottom: 1rem;
}

.todo--timer {
    font: 500 2rem 'Roboto', sans-serif;
    color: white;
    text-align: center;
    margin-bottom: 1rem;
}

</style>