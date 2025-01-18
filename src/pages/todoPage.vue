<template>
  <div class="todoPage">
    <div :class="['sectionOne', { 'menu-open': isMenuOpen }]">
      <sidebarMenu :tasks="taskArray" @delete-task="deleteTask" :selectedTask="selectedTask" @select-task="selectTask"
        @toggle-sidebar-form="toggleSidebarForm" :isMenuOpen="isMenuOpen" @toggle-menu="toggleMenu" />
    </div>

    <div class="sectionTwo">
      <transition name="fade">
        <SidebarForm v-if="isSidebarFormVisible" :selectedTask="selectedTask" @task-changed="handleTaskChange"
          @save-task="saveTask" />
      </transition>
    </div>
  </div>
</template>

<script setup>
import SidebarForm from '../components/sidebarForm.vue'
import sidebarMenu from '../components/sidebarMenu.vue'
import { ref } from 'vue';

const taskArray = ref([]);
const selectedTask = ref(null);
const isSidebarFormVisible = ref(false);
const isMenuOpen = ref(false);

const saveTask = (task) => {
  if (task.title && task.text) {
    const index = taskArray.value.findIndex(t => t.title === task.title);
    if (index !== -1) {
      taskArray.value[index] = task;
    } else {
      taskArray.value.push(task);
    }
    isSidebarFormVisible.value = false;
    selectedTask.value = null;
  }
};

const deleteTask = (index) => {
  taskArray.value.splice(index, 1);
};

const selectTask = (task) => {
  if (selectedTask.value && selectedTask.value.title === task.title) {
    isSidebarFormVisible.value = !isSidebarFormVisible.value;
  } else {
    selectedTask.value = task;
    isSidebarFormVisible.value = true;
  }
};

const toggleSidebarForm = () => {
  selectedTask.value = { title: '', text: '' };
  isSidebarFormVisible.value = true;
};

const handleTaskChange = (updatedTask) => {
  selectedTask.value = updatedTask;
};

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.todoPage {
  background: var(--page-color);
  display: flex;
  flex-direction: row;
  width: 100%;
  gap: 20px;
  height: 100%;
  padding: 13px;
}

.sectionTwo {
  flex: 1 1 auto;
}
</style>