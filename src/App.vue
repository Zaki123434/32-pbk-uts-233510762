<script setup>
import { ref, computed } from 'vue';

const tasks = ref([]);
const newTask = ref('');
const filter = ref('semua');

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      id: Date.now(),
      text: newTask.value,
      completed: false
    });
    newTask.value = '';
  }
};

const removeTask = (task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id);
};

const toggleTask = (task) => {
  task.completed == !task.completed;
}

const filteredTasks = computed(() => {
  if (filter.value === 'selesai') {
    return tasks.value.filter(task => task.completed);
  } else if (filter.value === 'belum selesai') {
    return tasks.value.filter(task => !task.completed);
  } else {
    return tasks.value;
  }
});

</script>

<template>
  <div>
    <input type="text" v-model="newTask" @keyup.enter="addTask">
    <button @click="addTask">Add Task</button>
    <select v-model="filter">
      <option value="semua">Semua</option>
      <option value="selesai">Selesai</option>
      <option value="belum selesai">Belum Selesai</option>
    </select>  
    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" @change="toggleTask(task)">
        {{ task.text }}
        <button @click="removeTask(task)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
