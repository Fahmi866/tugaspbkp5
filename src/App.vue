<template>
  <div class="container">
    <h1>{{ title }}</h1>

    <!-- Input tugas baru -->
    <div class="input-section">
      <input v-model="newTask" placeholder="Tambahkan tugas baru" @keyup.enter="addTask" />
      <button @click="addTask">Tambah</button>
    </div>

    <!-- Daftar tugas -->
    <ul>
      <li v-for="(task, index) in tasks" :key="index" :class="{ 'completed': task.completed }">
        {{ task.text }}
        <button @click="toggleTaskCompletion(index)">
          {{ task.completed ? 'Tandai Belum Selesai' : 'Tandai Selesai' }}
        </button>
        <button @click="removeTask(index)">Hapus</button>
      </li>
    </ul>

    <!-- Statistik tugas -->
    <p>Total tugas: {{ totalTasks }}</p>
    <p>Tugas selesai: {{ completedTasksCount }}</p>
    <p>Tugas belum selesai: {{ incompleteTasks }}</p>

    <!-- Pesan pengguna -->
    <div class="message-section">
      <input v-model="message" placeholder="Ketik pesan disini" />
      <button @click="showMessage">Tampilkan pesan</button>
      <p v-if="isMessageVisible">{{ message }}</p>
    </div>

    <!-- Highlight teks -->
    <div class="highlight-section">
      <p ref="highlightText" :class="{ 'highlight': isHighlighted }">Ini adalah teks dengan attribute binding.</p>
      <button @click="toggleHighlight">Toggle Highlight</button>
    </div>

    <!-- Status Lifecycle -->
    <div class="lifecycle-section">
      <p>Status Lifecycle: {{ lifecycleStatus }}</p>
      <p>Waktu sejak halaman dimuat: {{ timeElapsed }} detik</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted, onBeforeUnmount } from 'vue';

// Data Reaktif
const title = ref('Selamat Datang di Aplikasi Vue.js');
const message = ref('');
const isMessageVisible = ref(false);
const isHighlighted = ref(false);
const lifecycleStatus = ref('Sedang dimuat...');
const timeElapsed = ref(0);
const timerInterval = ref(null);
const newTask = ref('');
const highlightText = ref(null);

// Daftar tugas
const tasks = ref([
  { text: 'Belajar Vue.js', completed: false },
  { text: 'Membuat proyek dengan Vite', completed: false },
  { text: 'Memahami Composition API', completed: false }
]);

// Fungsi
function showMessage() {
  isMessageVisible.value = true;
}

function toggleHighlight() {
  isHighlighted.value = !isHighlighted.value;
}

function addTask() {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, completed: false });
    newTask.value = '';
  }
}

function removeTask(index) {
  tasks.value.splice(index, 1);
}

function toggleTaskCompletion(index) {
  tasks.value[index].completed = !tasks.value[index].completed;
}

// Computed Properties
const totalTasks = computed(() => tasks.value.length);
const completedTasksCount = computed(() => tasks.value.filter(task => task.completed).length);
const incompleteTasks = computed(() => totalTasks.value - completedTasksCount.value);

// Watchers
watch(tasks, (newTasks) => {
  console.log('Tasks diperbarui:', newTasks);
  if (completedTasksCount.value === totalTasks.value && totalTasks.value > 0) {
    alert('Selamat! Semua tugas telah diselesaikan!');
  }
}, { deep: true });

// Lifecycle Hooks
onMounted(() => {
  lifecycleStatus.value = 'Komponen telah dimount';
  console.log('Komponen telah dimount');
  timerInterval.value = setInterval(() => timeElapsed.value++, 1000);
});

onBeforeUnmount(() => {
  lifecycleStatus.value = 'Komponen akan dihapus';
  console.log('Komponen akan dihapus');
  clearInterval(timerInterval.value);
});
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.input-section, .message-section, .highlight-section, .lifecycle-section {
  margin-top: 20px;
  padding: 15px;
  border: 1px solid #ccc;
  border-radius: 8px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 10px 0;
  padding: 8px;
  border-bottom: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

button {
  margin-left: 8px;
  padding: 4px 8px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: 100%;
  margin-bottom: 10px;
}

.highlight {
  color: red;
  font-weight: bold;
  background-color: yellow;
  padding: 5px;
  border-radius: 4px;
}

.completed {
  text-decoration: line-through;
  color: green;
}
</style>
