<template>
  <div class="container">
    <h1>📚 StudyFlow</h1>
    <div class="input-section">
      <input 
        v-model="newTask" 
        @keyup.enter="addTask" 
        placeholder="Masukkan topik belajar (contoh: Baca Jurnal Sistem Cerdas)"
        class="task-input"
      >
      <button @click="addTask" class="add-btn">➕ Add</button>
    </div>

    <div class="filter-section">
      <button 
        @click="toggleFilter" 
        :class="{ 'active-filter': showActive }"
        class="filter-btn"
      >
        {{ showActive ? 'Tampilkan Semua' : 'Filter Aktif' }}
      </button>
    </div>

    <ul class="task-list">
      <li 
        v-for="(task, index) in filteredTasks" 
        :key="index" 
        :class="{ completed: task.completed }"
        class="task-item"
      >
        <div class="task-content">
          <input 
            type="checkbox" 
            v-model="task.completed" 
            class="checkbox"
          >
          <span class="task-text">{{ task.text }}</span>
        </div>
        <button @click="deleteTask(index)" class="delete-btn">🗑️</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTask = ref('')
const tasks = ref([])
const showActive = ref(false)

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({
      text: newTask.value.trim(),
      completed: false
    })
    newTask.value = ''
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}

const toggleFilter = () => {
  showActive.value = !showActive.value
}

const filteredTasks = computed(() => {
  return showActive.value 
    ? tasks.value.filter(task => !task.completed) 
    : tasks.value
})
</script>

<style>
body {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  margin: 0;
  padding: 20px;
  font-family: 'Segoe UI', sans-serif;
  color: #2d3748;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  background: rgba(255, 255, 255, 0.95);
  padding: 2rem;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
  color: #4a5568;
  margin-bottom: 2rem;
  font-size: 2.5rem;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 1.5rem;
}

.task-input {
  flex: 1;
  padding: 12px;
  border: 2px solid #cbd5e0;
  border-radius: 8px;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.task-input:focus {
  border-color: #667eea;
  outline: none;
}

.add-btn {
  background: #48bb78;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.add-btn:hover {
  transform: scale(1.05);
}

.filter-section {
  margin-bottom: 1.5rem;
  text-align: right;
}

.filter-btn {
  background: #4299e1;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
}

.active-filter {
  background: #f56565;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  margin-bottom: 0.5rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
  transition: all 0.3s ease;
}

.task-item:hover {
  transform: translateX(5px);
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

.task-content {
  display: flex;
  align-items: center;
  gap: 10px;
}

.checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.task-text {
  font-size: 1.1rem;
}

.completed .task-text {
  text-decoration: line-through;
  color: #a0aec0;
}

.delete-btn {
  background: #fc8181;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.delete-btn:hover {
  background: #f56565;
}

@media (max-width: 480px) {
  .container {
    padding: 1rem;
  }
  
  .input-section {
    flex-direction: column;
  }
  
  .add-btn {
    width: 100%;
  }
}
</style>