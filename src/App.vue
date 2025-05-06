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
  <div class="min-h-screen bg-gray-900 text-gray-100 p-5 flex flex-col">
    <header class="text-center text-3xl font-extrabold text-purple-400 mb-6">
      🌓 Daftar Tugas
    </header>

    <main class="flex-1 flex flex-col md:flex-row gap-6 h-120">
      <!-- Input Panel -->
      <div class="md:w-1/3 bg-gray-800 shadow-lg rounded-xl p-6 flex flex-col justify-between gap-4">
        <select
          v-model="filter"
          class="p-2 bg-gray-700 text-white rounded-md focus:outline-none focus:ring-2 focus:ring-purple-500"
        >
          <option value="semua">Semua</option>
          <option value="selesai">Selesai</option>
          <option value="belum selesai">Belum Selesai</option>
        </select>
        <div class="flex flex-col gap-4">
          <input
          type="text"
          v-model="newTask"
          @keyup.enter="addTask"
          placeholder="Tambahkan tugas baru"
          class="p-3 rounded-md bg-gray-700 text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-purple-500"
          />
          <button
          @click="addTask"
          class="bg-purple-600 hover:bg-purple-700 text-white px-4 py-2 rounded-md transition"
          >
          Tambah Tugas
        </button>
      </div>

      </div>

      <!-- List Panel -->
      <div class="flex-1 bg-gray-800 shadow-lg rounded-xl p-5 overflow-y-auto h-120">
        <ul class="space-y-3">
          <li
            v-for="task in filteredTasks"
            :key="task.id"
            class="flex items-center justify-between bg-gray-700 p-3 rounded-md"
          >
            <div class="flex items-center gap-3">
              <input
                type="checkbox"
                v-model="task.completed"
                class="accent-purple-500 w-5 h-5"
              />
              <span :class="{ 'line-through text-gray-400': task.completed }">
                {{ task.text }}
              </span>
            </div>
            <button
              @click="removeTask(task)"
              class="text-red-400 hover:text-red-600 font-bold"
            >
              ✕
            </button>
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>
