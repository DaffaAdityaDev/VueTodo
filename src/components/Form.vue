<template>
    <form @submit.prevent="addItem">
        <h1 class="todo--title">Enter Your Todo</h1>
        <input type="text" v-model="form.name" class="todo--input" placeholder="TODO"/>
        <div class="todo--timeselector">
            <input type="number" v-model="form.timeSecond" class="todo--input" placeholder="Time In Second"/>
            <input type="number" v-model="form.timeMinute" class="todo--input" placeholder="Time In Minute"/>
            <input type="number" v-model="form.timeHour" class="todo--input" placeholder="Time In Hour"/>
        </div>
        <button type="submit" class="todo--button">Add</button>
    </form>
</template>


<script setup>
import { ref } from 'vue';

const form = ref({
    name: '',
    timeSecond: '',
    timeMinute: '',
    timeHour: '',
});

const addItem = () => {
    document.dispatchEvent(
        new CustomEvent('addItem', {
        detail: {
            name: form.value.name,
            time: form.value.timeSecond + form.value.timeMinute + form.value.timeHour,
        },
        })
    );
    form.value.name = '';
    form.value.timeSecond = '';
    form.value.timeMinute = '';
    form.value.timeHour = '';
};


</script>


<style scoped>

.todo--title {
    font: 500 2rem 'Roboto', sans-serif;
    color: white;
    text-align: center;
    margin-bottom: 1rem;
}

.todo--timeselector {
    display: flex;
    gap: 1rem;
    justify-content: space-between;
    align-items: center;
    margin-top: 1rem;
    
}

.todo--input::placeholder {
    font-size: 1rem; /* Adjust this value to your liking */
    text-align: center;
}

.todo--input {
    width: 100%;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 10px;
    margin-top: 1rem;
    color: black;
    background: white;
    font: 500 1.5rem 'Roboto', sans-serif;
    box-sizing: border-box;
    transition: box-shadow 0.3s ease-in-out;
}

.todo--input:focus {
    outline: none;
    box-shadow: 0 0 0 3px limegreen;
}

.todo--button {
    width: 100%;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 10px;
    margin-top: 1rem;
    color: white;
    background: rgba(128,236,166);
    background: linear-gradient(36deg, rgba(128,236,166,0.7) 0%, rgba(0,255,42, 0.5) 44%, rgba(242,255,113, 0.7) 100%);
    font: 500 1.5rem 'Roboto', sans-serif;
    box-sizing: border-box;
    transition: box-shadow 0.3s ease-in-out;
    margin-bottom: 3rem;
}

</style>
