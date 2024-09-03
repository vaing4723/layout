<!-- components/SideWindow.vue -->
<template>
    <div class="side-window">
      <h3>侧边窗口</h3>
      <div class="content">{{ window.content }}</div>
    </div>
  </template>
  
  <script setup>
  defineProps(['window'])
  </script>
  
  <style scoped>
  .side-window {
    border: 1px solid #000;
    padding: 5px;
    box-sizing: border-box;
  }
  
  .content {
    font-size: 14px;
  }
  </style>