<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';
import TodoItem from './TodoItem.vue';

interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

// 待办事项列表
const todos = ref<Todo[]>([]);
// 新待办事项输入
const newTodo = ref('');
// 过滤类型："all", "active", "completed"
const filter = ref('all');
// 加载状态
const isLoading = ref(true);

// 过滤后的待办事项
const filteredTodos = computed(() => {
  switch (filter.value) {
    case 'active':
      return todos.value.filter(todo => !todo.completed);
    case 'completed':
      return todos.value.filter(todo => todo.completed);
    default:
      return todos.value;
  }
});

// 计算完成和未完成的数量
const stats = computed(() => {
  const total = todos.value.length;
  const completed = todos.value.filter(todo => todo.completed).length;
  const active = total - completed;
  return { total, completed, active };
});

// 从本地存储加载待办事项
onMounted(() => {
  const savedTodos = localStorage.getItem('todos');
  if (savedTodos) {
    todos.value = JSON.parse(savedTodos);
  }
  // 模拟加载延迟
  setTimeout(() => {
    isLoading.value = false;
  }, 500);
});

// 保存到本地存储
function saveTodos() {
  localStorage.setItem('todos', JSON.stringify(todos.value));
}

// 添加新的待办事项
function addTodo() {
  if (newTodo.value.trim()) {
    const id = Date.now();
    todos.value.push({
      id,
      text: newTodo.value.trim(),
      completed: false
    });
    newTodo.value = '';
    saveTodos();
  }
}

// 更新待办事项状态
function updateTodoStatus(id: number, completed: boolean) {
  const todo = todos.value.find(todo => todo.id === id);
  if (todo) {
    todo.completed = completed;
    saveTodos();
  }
}

// 编辑待办事项
function editTodo(id: number, text: string) {
  const todo = todos.value.find(todo => todo.id === id);
  if (todo) {
    todo.text = text;
    saveTodos();
  }
}

// 删除待办事项
function deleteTodo(id: number) {
  todos.value = todos.value.filter(todo => todo.id !== id);
  saveTodos();
}

// 清除已完成的待办事项
function clearCompleted() {
  todos.value = todos.value.filter(todo => !todo.completed);
  saveTodos();
}

// 全选/取消全选
function toggleAll() {
  const allCompleted = todos.value.every(todo => todo.completed);
  todos.value.forEach(todo => {
    todo.completed = !allCompleted;
  });
  saveTodos();
}
</script>

<template>
  <div class="todo-container">
    <div class="todo-header">
      <h1>待办事项</h1>
      <div class="new-todo-form">
        <input
          v-model="newTodo"
          @keyup.enter="addTodo"
          placeholder="添加新的待办事项..."
          class="new-todo-input"
        />
        <button @click="addTodo" class="add-todo-btn">添加</button>
      </div>
    </div>
    
    <div class="todo-filters">
      <button 
        @click="filter = 'all'" 
        :class="{ active: filter === 'all' }"
        class="filter-btn"
      >
        全部
      </button>
      <button 
        @click="filter = 'active'" 
        :class="{ active: filter === 'active' }"
        class="filter-btn"
      >
        未完成
      </button>
      <button 
        @click="filter = 'completed'" 
        :class="{ active: filter === 'completed' }"
        class="filter-btn"
      >
        已完成
      </button>
    </div>

    <div v-if="isLoading" class="todo-loading">
      <div class="loading-spinner"></div>
      <p>加载中...</p>
    </div>
    
    <div v-else-if="todos.length === 0" class="empty-state">
      <div class="empty-icon">📝</div>
      <p>还没有待办事项</p>
      <p class="empty-hint">添加一些任务开始使用吧</p>
    </div>
    
    <div v-else>
      <div v-if="filteredTodos.length > 0" class="todos-list">
        <div v-if="todos.length > 1" class="toggle-all-container">
          <button @click="toggleAll" class="toggle-all-btn">
            {{ todos.every(todo => todo.completed) ? '取消全选' : '全选' }}
          </button>
        </div>
        
        <TransitionGroup name="todo-list">
          <TodoItem
            v-for="todo in filteredTodos"
            :key="todo.id"
            :todo="todo"
            @update:completed="(completed) => updateTodoStatus(todo.id, completed)"
            @delete="deleteTodo"
            @edit="editTodo"
          />
        </TransitionGroup>
      </div>
      
      <div v-else class="empty-filtered">
        <p>{{ filter === 'active' ? '没有未完成的待办事项' : '没有已完成的待办事项' }}</p>
      </div>
      
      <div v-if="todos.length > 0" class="todo-footer">
        <div class="todo-stats">
          <span>{{ stats.active }} 项未完成</span>
          <span>{{ stats.completed }} 项已完成</span>
        </div>
        <button 
          v-if="stats.completed > 0"
          @click="clearCompleted" 
          class="clear-completed-btn"
        >
          清除已完成
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.todo-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.todo-header {
  margin-bottom: 24px;
  text-align: center;
}

.todo-header h1 {
  font-size: 32px;
  margin-bottom: 16px;
  color: #3498db;
  font-weight: 700;
}

.new-todo-form {
  display: flex;
  margin-bottom: 16px;
}

.new-todo-input {
  flex: 1;
  padding: 12px 16px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 8px 0 0 8px;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.new-todo-input:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 0 2px rgba(52, 152, 219, 0.2);
}

.add-todo-btn {
  padding: 12px 24px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 0 8px 8px 0;
  cursor: pointer;
  font-weight: 600;
  transition: background-color 0.3s;
}

.add-todo-btn:hover {
  background-color: #2980b9;
}

.todo-filters {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
  gap: 8px;
}

.filter-btn {
  padding: 8px 16px;
  background-color: #f8f9fa;
  border: 1px solid #ddd;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s;
}

.filter-btn:hover {
  background-color: #e9ecef;
}

.filter-btn.active {
  background-color: #3498db;
  color: white;
  border-color: #3498db;
}

.todos-list {
  margin-bottom: 16px;
}

.toggle-all-container {
  margin-bottom: 12px;
  text-align: right;
}

.toggle-all-btn {
  padding: 6px 12px;
  background-color: transparent;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  color: #666;
}

.toggle-all-btn:hover {
  background-color: #f8f9fa;
}

.todo-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 16px;
  border-top: 1px solid #eee;
  font-size: 14px;
  color: #666;
}

.todo-stats {
  display: flex;
  gap: 16px;
}

.clear-completed-btn {
  padding: 6px 12px;
  background-color: transparent;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  color: #e74c3c;
  transition: all 0.3s;
}

.clear-completed-btn:hover {
  background-color: #fee9e9;
  border-color: #e74c3c;
}

.empty-state, .empty-filtered, .todo-loading {
  text-align: center;
  padding: 40px 0;
  color: #777;
}

.empty-icon {
  font-size: 48px;
  margin-bottom: 16px;
}

.empty-hint {
  font-size: 14px;
  color: #999;
  margin-top: 8px;
}

.loading-spinner {
  display: inline-block;
  width: 40px;
  height: 40px;
  border: 4px solid rgba(52, 152, 219, 0.1);
  border-radius: 50%;
  border-top-color: #3498db;
  animation: spin 1s ease-in-out infinite;
  margin-bottom: 16px;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* 列表动画 */
.todo-list-enter-active,
.todo-list-leave-active {
  transition: all 0.5s ease;
}
.todo-list-enter-from,
.todo-list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style> 