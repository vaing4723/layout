<!-- App.vue -->
<template>
  <div class="app-container">
    <div class="main-area">
      <component
        :is="mainWindow.component"
        :key="mainWindow.id"
        :window="mainWindow"
        class="window main-window"
        @dblclick="handleMainWindowDblClick"
      />
    </div>
    <div class="side-area">
      <!-- <component
        v-for="window in sideWindows"
        :key="window.id"
        :is="window.component"
        :window="window"
        class="window side-window"
        @dblclick="handleSideWindowDblClick(window.id)"
      /> -->
      <component
        :is="topWindow.component"
        :key="topWindow.id"
        :window="topWindow"
        class="window side-window"
        @dblclick="handleSideWindowDblClick(topWindow.id)"
      />
      <component
        :is="middleWindow.component"
        :key="middleWindow.id"
        :window="middleWindow"
        class="window side-window"
        @dblclick="handleSideWindowDblClick(middleWindow.id)"
      />
      <component
        :is="bottomWindow.component"
        :key="bottomWindow.id"
        :window="bottomWindow"
        class="window side-window"
        @dblclick="handleSideWindowDblClick(bottomWindow.id)"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, markRaw } from 'vue'
import MainWindow from './components/MainWindow.vue'
import SideWindow from './components/SideWindow.vue'

const windows = ref([
  { id: 1, component: markRaw(MainWindow), content: '主窗口内容主窗口内容主窗口内容主窗口内容主窗口内容', position: 'main' },
  { id: 2, component: markRaw(SideWindow), content: '右上窗口内容', position: 'top' },
  { id: 3, component: markRaw(SideWindow), content: '右中窗口内容', position: 'middle' },
  { id: 4, component: markRaw(SideWindow), content: '右下窗口内容', position: 'bottom' }
])

const mainWindow = computed(() => windows.value.find(w => w.position === 'main'))
const topWindow = computed(() => windows.value.find(w => w.position === 'top'))
const middleWindow = computed(() => windows.value.find(w => w.position === 'middle'))
const bottomWindow = computed(() => windows.value.find(w => w.position === 'bottom'))

// const sideWindows = computed(() => windows.value.filter(w => w.position !== 'main'))

const swapWindows = (id) => {
  const mainIndex = windows.value.findIndex(w => w.position === 'main')
  const targetIndex = windows.value.findIndex(w => w.id === id)
  
  if (mainIndex !== -1 && targetIndex !== -1) {
    const mainPosition = windows.value[mainIndex].position
    windows.value[mainIndex].position = windows.value[targetIndex].position
    windows.value[targetIndex].position = mainPosition
  }
}

const handleSideWindowDblClick = (id) => {
  const clickedWindow = windows.value.find(w => w.id === id)
  
  // if (clickedWindow.position === 'bottom') {
    const currentMain = windows.value.find(w => w.position === 'main')
    if (currentMain.id !== 1) { // If main is not the bottom window
      swapWindows(id)
    } else {
      // If main is the bottom window, swap it back to its original position
      const originalBottom = windows.value.find(w => w.position === 'bottom')
      swapWindows(originalBottom.id)
      swapWindows(id);
    }
  // } else {
  //   swapWindows(id)
  // }
}

const handleMainWindowDblClick = () => {
  // 主窗口双击时不执行任何操作
}
</script>

<style scoped>
.app-container {
  display: flex;
  height: 100vh;
  width: 100vw;
}

.main-area {
  width: 75%;
  height: 100%;
}

.side-area {
  width: 25%;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.window {
  box-sizing: border-box;
  border: 1px solid #ccc;
  padding: 10px;
  overflow: auto;
}

.main-window {
  width: 100%;
  height: 100%;
}

.side-window {
  width: 100%;
  height: 33.33%;
}

.main-area .side-window {
  width: 100%;
  height: 100%;
}
</style>

<!-- MainWindow.vue 和 SideWindow.vue 保持不变 -->