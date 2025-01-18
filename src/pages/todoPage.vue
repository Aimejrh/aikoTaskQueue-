<template>
  <div class="todoPage">
    <div :class="['sectionOne', { 'menu-open': isMenuOpen }]">
      <sidebarMenu :tasks="taskArray" @delete-task="deleteTask" :selectedTask="selectedTask" @select-task="selectTask"
        @toggle-sidebar-form="toggleSidebarForm" :isMenuOpen="isMenuOpen" @toggle-menu="toggleMenu" />
    </div>

    <div class="sectionTwo">
      <transition name="fade">
        <SidebarForm v-if="isSidebarForm" :selectedTask="selectedTask" @task-changed="handleTaskChange"
          @save-task="saveTask" />
      </transition>
    </div>
  </div>
</template>

<script>
import sidebarMenu from '../components/sidebarMenu.vue';
import SidebarForm from '../components/sidebarForm.vue';
export default {
  components: {
    sidebarMenu,
    SidebarForm,
  },
  data() {
    return {
      taskArray: [],
      selectedTask: null,
      isSidebarForm: false,
      isMenuOpen: false,
    };
  },
  methods: {
    saveTask(task) {
      if (task.title && task.text) {
        const index = this.taskArray.findIndex(t => t.title === task.title);
        if (index !== -1) {
          this.taskArray[index] = task;
        } else {
          this.taskArray.push(task);
        }
        this.isSidebarForm = false;
        this.selectedTask = null;
      }
    },
    deleteTask(index) {
      this.taskArray.splice(index, 1);
    },
    selectTask(task) {
      if (task.title && task.text) {
        const index = this.taskArray.findIndex(t => t.id === task.id);
        if (index !== -1) {
          this.taskArray[index] = task;
        } else {
          this.taskArray.push({ ...task, id: Date.now() });
        }
        this.isSidebarForm = false;
        this.selectedTask = null;
      }
    },
    toggleSidebarForm() {
      this.selectedTask = { title: '', text: '' };
      this.isSidebarForm = true;
    },
    handleTaskChange(updatedTask) {
      this.selectedTask = updatedTask;
    },
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
  },
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
  background-image: url("https://sotni.ru/wp-content/uploads/2023/08/kirpichnaia-stena-s-podsvetkoi-35.webp");
  background-size: cover;
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