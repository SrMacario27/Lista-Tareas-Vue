<script setup>
defineProps({
  task: {
    type: Object,
    required: true
  }
})

defineEmits(['toggle-task', 'delete-task'])
</script>

<template>
  <li class="task">
    <label>
      <input
        type="checkbox"
        :checked="task.completed"
        @change="$emit('toggle-task', task.id)"
      />

      <span :class="{ completed: task.completed }">
        {{ task.text }}
      </span>
    </label>

    <button @click="$emit('delete-task', task.id)">
      Eliminar
    </button>
  </li>
</template>

<style scoped>
.task {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 12px;
  margin-bottom: 10px;
  background: #f1f5f9;
  border-radius: 8px;
}

.completed {
  color: #64748b;
  text-decoration: line-through;
}

button {
  padding: 8px 12px;
  border: none;
  border-radius: 6px;
  color: white;
  background: #dc2626;
  cursor: pointer;
}
</style>