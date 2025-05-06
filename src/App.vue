<script setup>
import { ref } from 'vue';

const tasks = ref([]);
const newTask = ref('');

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({ text: newTask.value, completed: false });
    newTask.value = '';
  }
};

</script>

<template>
  <div>
    <input type="text" v-model="newTask" @keyup.enter="addTask">
    <button @click="addTask">Add Task</button>

    <ul>
      <li v-for="task in tasks" :key="task.id">
        {{ task.text }}
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
