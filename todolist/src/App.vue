<script setup lang="ts">
import TodoList from './components/TodoList.vue'
</script>

<template>
  <div class="app">
    <TodoList />
  </div>
</template>

<style>
/* 全局样式 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'PingFang SC', 'Helvetica Neue', Arial, sans-serif;
  color: #333;
  background-color: #f5f7fa;
  line-height: 1.6;
}

.app {
  min-height: 100vh;
  padding: 40px 20px;
}

button {
  font-family: inherit;
}

@media (max-width: 768px) {
  .app {
    padding: 20px 10px;
  }
}

/* 适配深色模式 */
@media (prefers-color-scheme: dark) {
  body {
    background-color: #1a1a1a;
    color: #f5f5f5;
  }
}
</style>
