<template>
  <div class="container">
    <h1>Gestor de tareas</h1>

    <!-- Input con v-model -->
    <input 
      v-model="newTask" 
      placeholder="Escribe el nombre de la tarea" 
      @keyup.enter="addTask" 
    />
    <button @click="addTask">Agregar</button>

    <hr />

    <!-- v-if / v-else -->
    <div v-if="tasks.length > 0">
      <h2>Listado de tareas</h2>

      <!-- Tablero estilo Kanban -->
      <div class="board">
        <div class="column">
          <h3>To Do</h3>
          <div 
            v-for="(task, index) in tasks.filter(t => t.status === 'todo')" 
            :key="index" 
            class="task blue"
          >
            {{ task.name }}
            <button @click="moveTask(index, 'doing')">➡</button>
          </div>
        </div>

        <div class="column">
          <h3>Doing</h3>
          <div 
            v-for="(task, index) in tasks.filter(t => t.status === 'doing')" 
            :key="index" 
            class="task green"
          >
            {{ task.name }}
            <button @click="moveTask(index, 'done')">➡</button>
          </div>
        </div>

        <div class="column">
          <h3>Done</h3>
          <div 
            v-for="(task, index) in tasks.filter(t => t.status === 'done')" 
            :key="index" 
            class="task red"
          >
            {{ task.name }}
            <button @click="removeTask(index)">X</button>
          </div>
        </div>
      </div>
    </div>

    <div v-else>
      <p>No hay tareas registradas.</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Modelo
const newTask = ref('')
const tasks = ref([])

// Lógica
const addTask = () => {
  const name = newTask.value.trim()
  if (name !== '') {
    tasks.value.push({ name, status: 'todo' })
    newTask.value = ''
  }
}

const moveTask = (index, newStatus) => {
  tasks.value[index].status = newStatus
}

const removeTask = (index) => {
  tasks.value.splice(index, 1)
}
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 40px auto;
  font-family: Arial, sans-serif;
  text-align: center;
}

input {
  padding: 8px;
  width: 60%;
}

button {
  margin-left: 8px;
  padding: 6px 10px;
  cursor: pointer;
}

.board {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.column {
  width: 30%;
  border: 1px dashed #ccc;
  padding: 10px;
  border-radius: 6px;
  min-height: 200px;
}

.task {
  margin: 6px 0;
  padding: 6px;
  border-radius: 4px;
  color: white;
  display: flex;
  justify-content: space-between;
}

.blue { background: #3498db; }
.green { background: #27ae60; }
.red { background: #e74c3c; }
</style>
