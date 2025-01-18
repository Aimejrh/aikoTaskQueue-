<template>
  <div class="todoPage">
    <div :class="['sectionOne', { 'menu-open': isMenuOpen }]">
      <sidebarMenu :tasks="taskArray" :selectedTask="selectedTask" :toggleMenu="toggleMenu"
        :toggleSidebarForm="toggleSidebarForm" :selectTask="selectTask" :deleteTask="deleteTask"
        :isMenuOpen="isMenuOpen" />

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
import SidebarForm from '../components/sidebarForm.vue';
import sidebarMenu from '../components/sidebarMenu.vue';

export default {
  components: {
    SidebarForm,
    sidebarMenu,
  },
  data() {
    return {
      taskArray: [],
      selectedTask: null,
      isSidebarForm: false,
      isMenuOpen: true,
    };
  },
  methods: {
    saveTask(task) {
      if (task.title && task.text) {
        const index = this.taskArray.findIndex(t => t.id === task.id);
        if (index !== -1) {
          this.taskArray[index] = task;
        } else {
          task.id = Date.now();
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
      if (this.selectedTask && this.selectedTask.id === task.id) {
        this.isSidebarForm = !this.isSidebarForm;
      } else {
        this.selectedTask = { ...task };
        this.isSidebarForm = true;
      }
    },
    toggleSidebarForm() {
      this.selectedTask = { title: '', text: '', id: Date.now() };
      this.isSidebarForm = true;
    },
    handleTaskChange(updatedTask) {
      this.selectedTask = updatedTask;
    },
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    }
  }
}
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