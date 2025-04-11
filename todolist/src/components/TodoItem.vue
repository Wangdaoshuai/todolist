<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps<{
  todo: {
    id: number;
    text: string;
    completed: boolean;
  };
}>();

const emit = defineEmits<{
  'update:completed': [completed: boolean];
  'delete': [id: number];
  'edit': [id: number, text: string];
}>();

const isEditing = ref(false);
const editText = ref(props.todo.text);

function toggleComplete() {
  emit('update:completed', !props.todo.completed);
}

function deleteTodo() {
  emit('delete', props.todo.id);
}

function startEditing() {
  isEditing.value = true;
  editText.value = props.todo.text;
}

function finishEditing() {
  if (editText.value.trim()) {
    emit('edit', props.todo.id, editText.value);
    isEditing.value = false;
  }
}

function cancelEditing() {
  isEditing.value = false;
  editText.value = props.todo.text;
}
</script>

<template>
  <div class="todo-item" :class="{ completed: todo.completed }">
    <div v-if="!isEditing" class="todo-content">
      <div class="todo-actions">
        <input 
          type="checkbox" 
          :checked="todo.completed" 
          @change="toggleComplete"
          class="todo-checkbox"
        />
        <span class="todo-text">{{ todo.text }}</span>
      </div>
      <div class="todo-buttons">
        <button @click="startEditing" class="btn edit-btn">
          ✏️
        </button>
        <button @click="deleteTodo" class="btn delete-btn">
          🗑️
        </button>
      </div>
    </div>
    <div v-else class="todo-edit">
      <input 
        v-model="editText"
        @keyup.enter="finishEditing"
        @keyup.esc="cancelEditing"
        class="edit-input"
        ref="editInput"
        autoFocus
      />
      <div class="edit-buttons">
        <button @click="finishEditing" class="btn save-btn">
          ✓
        </button>
        <button @click="cancelEditing" class="btn cancel-btn">
          ✗
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.todo-item {
  display: flex;
  padding: 12px 16px;
  background-color: white;
  border-radius: 8px;
  margin-bottom: 12px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.todo-item:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  transform: translateY(-2px);
}

.todo-content, .todo-edit {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.todo-actions {
  display: flex;
  align-items: center;
  flex: 1;
}

.todo-checkbox {
  margin-right: 12px;
  height: 18px;
  width: 18px;
  cursor: pointer;
}

.todo-text {
  font-size: 16px;
  color: #333;
  word-break: break-word;
}

.completed .todo-text {
  text-decoration: line-through;
  color: #888;
}

.todo-buttons, .edit-buttons {
  display: flex;
  gap: 8px;
}

.btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border: none;
  border-radius: 4px;
  background-color: transparent;
  cursor: pointer;
  transition: background-color 0.2s;
  font-size: 16px;
}

.edit-btn:hover { background-color: #e9f5fe; color: #3498db; }
.delete-btn:hover { background-color: #fee9e9; color: #e74c3c; }
.save-btn:hover { background-color: #eafaf1; color: #2ecc71; }
.cancel-btn:hover { background-color: #fee9e9; color: #e74c3c; }

.edit-input {
  flex: 1;
  padding: 8px 12px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-right: 8px;
}

.edit-input:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 0 2px rgba(52, 152, 219, 0.2);
}
</style> 