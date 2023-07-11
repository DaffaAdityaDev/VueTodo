<template>
    <div :class="['item', { 'done': item.done }]">
        <div class="details">
            <div>
                <h3 :class="{'crossed-out': item.done}">{{ item.text }}</h3>
                <p>Time : {{ formatTime(item.time) }}</p>    
            </div>
            <div class="container--control">
                <button class="done" v-on:click="markDone">Done</button>
                <button class="delete" v-on:click="deleteItem">Delete</button>
                <button class="startTime" v-on:click="startTime">Start</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  item: {
    type: Object,
    required: true
  }
});

const formatTime = (time) => {
  const hour = Math.floor(time / 3600);
  const minute = Math.floor((time % 3600) / 60);
  const second = Math.floor((time % 3600) % 60);
  return `${hour}:${minute}:${second}`;
};

const markDone = () => {
  document.dispatchEvent(
    new CustomEvent('markDone', {
      detail: {
        id: props.item.id,
      },
    })
  );
};


const deleteItem = () => {
  document.dispatchEvent(
    new CustomEvent('deleteItem', {
      detail: {
        id: props.item.id,
      },
    })
  );
};

const startTime = () => {
  document.dispatchEvent(
    new CustomEvent('startTime', {
      detail: {
        id: props.item.id,
        time: props.item.time,
      },
    })
  );
};


</script>

<style scoped>

.item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 15px;
    margin-top: 1rem;
    color: white;
    background: rgba(128,236,166);
    background: linear-gradient(36deg, rgba(128,236,166,0.7) 0%, rgba(0,255,42, 0.5) 44%, rgba(242,255,113, 0.7) 100%);
}

.item.done {
    background: rgba(128,236,166);
    background: linear-gradient(36deg, rgba(128,236,166,0.3) 0%, rgba(0,255,42, 0.2) 44%, rgba(242,255,113, 0.3) 100%);
}

.details {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
}

.container--control {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

.container--control button {
    padding: 0.5rem 1rem;
    border-radius: 15px;
    border: none;
    color: white;
    font-weight: bold;
    cursor: pointer;
}

.container--control button.done {
    background:limegreen;
}

.container--control button.delete {
    background:crimson;
}

.item .details h3.crossed-out {
  text-decoration: line-through;
}

.startTime {
    background: rgb(18, 52, 114);
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 15px;
    border: none;
    font-weight: bold;
    cursor: pointer;
}

</style>
