<!-- components/MainWindow.vue -->
<template>
    <div class="main-window">
      <h2>主窗口</h2>
      <div class="content">{{ window.content }}</div>
    </div>
  </template>
  
  <script setup>
  import { watch, ref } from 'vue';
  
  // 接收 props
  const props = defineProps(['window']);
  
  // 响应式数据
  const position = ref(props.window.position);
  
  // 监听 props 的变化
  watch(() => props.window.position, (newContent) => {
    position.value = newContent;
    update();
  }, { immediate: true });
  
  function update() {
    console.log('MainWindow updated:', position.value);
  }
  </script>
  
  <style scoped>
  .main-window {
    border: 1px solid #000;
    padding: 10px;
    box-sizing: border-box;
  }
  
  .content {
    font-size: 24px;
  }
  </style>
  