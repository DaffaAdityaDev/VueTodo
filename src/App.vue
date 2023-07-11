<script setup>

import HelloWorld from './components/HelloWorld.vue'
import Form from './components/Form.vue'
import Timer from './components/Timer.vue'
import ListItem from './components/ListItem.vue'
import { onMounted, ref, watch } from 'vue';

const TodoList = ref([
  { id: 1, text: 'Learn Vue 3', done: true, time: 30 },
  { id: 2, text: 'Learn React', done: false, time: 1 },
  { id: 3, text: 'Learn Angular', done: false, time: 10 },
]);

const currentSelectedTimer = ref(0);
const currentTodo = ref(null);
const intervalId = ref(null);

const addTodo = (text, time) => {
  TodoList.value.push({
    id: TodoList.value.length + 1,
    text,
    done: false,
    time: time,
  });
};

const handleTimer = (id) => {
  if (intervalId.value) {
    clearInterval(intervalId.value);
  }
  
  intervalId.value = setInterval(() => {
    currentSelectedTimer.value--;
    if (currentSelectedTimer.value === 0) {
      clearInterval(intervalId.value);
      document.dispatchEvent(
        new CustomEvent('markDone', {
          detail: {
            id: id,
          },
        })
      );
      alert('Time is up!');
    }
  }, 1000);

  watch(currentSelectedTimer, (newValue) => {
    if (newValue === 0) {
      clearInterval(intervalId.value);
    }
  });

};



onMounted(() => {
  document.addEventListener('addItem', (e) => {
    console.log(TodoList.value)
    if (!e.detail.name || e.detail.name.trim() === "") return;
    addTodo(
      e.detail.name,
      e.detail.time
    );
  });

  document.addEventListener('deleteItem', (e) => {
    console.log(e)
    TodoList.value = TodoList.value.filter((item) => item.id !== e.detail.id);
  });

  document.addEventListener('markDone', (e) => {
    console.log(e)
    TodoList.value = TodoList.value.map((item) => {
      if (item.id === e.detail.id) {
        item.done = true;
      }
      return item;
    });
  });

  document.addEventListener('startTime', (e) => {
    console.log(e.detail.time)
    currentSelectedTimer.value = e.detail.time;
    currentTodo.value = TodoList.value.find((item) => item.id === e.detail.id);
    handleTimer(e.detail.id);
  });

})


</script>

<template>


  <main>
    <HelloWorld msg="Todo List" />
    <Form />
    <Timer :time="currentSelectedTimer" :todo="currentTodo"/>
    <ListItem v-for="item in TodoList" :key="item.id" :item="item" />
  </main>
</template>


<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  /* header { */
    /* display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  } */

  .logo {
    margin: 0 2rem 0 0;
  }

  /* header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  } */
}
</style>
