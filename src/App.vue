<script setup>
import { computed, ref } from 'vue'
import TaskItem from './components/TaskItem.vue'

// Estado reactivo: texto que el usuario escribe.
const newTask = ref('')

// Estado reactivo: tareas de la aplicación.
const tasks = ref([
  {
    id: 1,
    text: 'Aprender fundamentos de Vue.js',
    completed: false
  },
  {
    id: 2,
    text: 'Subir el proyecto a GitHub',
    completed: false
  }
])

// Valor calculado: cuenta las tareas completadas.
const completedTasks = computed(() => {
  return tasks.value.filter(task => task.completed).length
})

// Agrega una tarea al presionar el botón o Enter.
function addTask() {
  const text = newTask.value.trim()

  if (text === '') {
    return
  }

  tasks.value.push({
    id: Date.now(),
    text: text,
    completed: false
  })

  newTask.value = ''
}

// Cambia una tarea entre completada y pendiente.
function toggleTask(id) {
  const task = tasks.value.find(task => task.id === id)

  if (task) {
    task.completed = !task.completed
  }
}

// Borra una tarea.
function deleteTask(id) {
  tasks.value = tasks.value.filter(task => task.id !== id)
}
</script>

<template>
  <main class="container">
    <section class="card">
      <h1>Lista de tareas</h1>
      <p class="description">
        Organiza tus actividades con Vue.js.
      </p>

      <form @submit.prevent="addTask">
        <input
          v-model="newTask"
          type="text"
          placeholder="Escribe una tarea"
          aria-label="Nueva tarea"
        />

        <button type="submit">Agregar</button>
      </form>

      <p class="counter">
        Completadas: {{ completedTasks }} de {{ tasks.length }}
      </p>

      <ul v-if="tasks.length > 0">
        <TaskItem
          v-for="task in tasks"
          :key="task.id"
          :task="task"
          @toggle-task="toggleTask"
          @delete-task="deleteTask"
        />
      </ul>

      <p v-else class="empty">
        No hay tareas. Agrega una nueva.
      </p>
    </section>
  </main>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
  background: linear-gradient(135deg, #dbeafe, #ede9fe);
}

.container {
  display: grid;
  min-height: 100vh;
  padding: 24px;
  place-items: center;
}

.card {
  width: 100%;
  max-width: 620px;
  padding: 32px;
  background: white;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(15, 23, 42, 0.15);
}

h1 {
  margin: 0;
  color: #1e3a8a;
}

.description {
  color: #475569;
}

form {
  display: flex;
  gap: 10px;
  margin: 24px 0 16px;
}

input[type="text"] {
  flex: 1;
  min-width: 0;
  padding: 12px;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  font-size: 16px;
}

form button {
  padding: 12px 16px;
  border: none;
  border-radius: 6px;
  color: white;
  background: #2563eb;
  font-weight: bold;
  cursor: pointer;
}

.counter {
  color: #475569;
  font-weight: bold;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

.empty {
  padding: 16px;
  text-align: center;
  color: #64748b;
  background: #f8fafc;
  border-radius: 8px;
}

@media (max-width: 500px) {
  form {
    flex-direction: column;
  }
}
</style>