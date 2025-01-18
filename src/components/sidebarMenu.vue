<template>
  <aside :class="['sidebar']" :style="{ width: isMenuOpen ? '310px' : '60px' }">
    <div class="tasksContainer">
      <header class="sidebarHeader">
        <button class="toggleCrossButton" @click="toggleMenu">
          <img class="toggleCross" src="../../public/Vector.svg" alt="menu">
        </button>
      </header>

      <ul :style="{ display: isMenuOpen ? 'flex' : 'none' }" class="sidebarUl">
        <li @click="selectTask(task)" v-for="(task, index) in tasks" :key="task.id" class="sidebarLi"
          :class="{ selected: selectedTask && selectedTask.title === task.title }">
          <span class="taskTitle">{{ task.title }}</span>
          <button @click.stop="deleteTask(index)" class="deleteButton">
            <img src="../../public/Vector.svg" alt="delete">
          </button>
        </li>
      </ul>
    </div>

    <button :style="{ display: isMenuOpen ? 'flex' : 'none' }" @click="toggleSidebarForm" class="createButton">
      Create
    </button>
  </aside>
</template>

<script>
export default {
  props: {
    tasks: {
      type: Array,
      required: true,
    },
    selectedTask: {
      type: Object,
      default: null,
    },
    isMenuOpen: {
      type: Boolean,
      default: true,
    },
    toggleMenu: {
      type: Function,
      required: true,
    },
    toggleSidebarForm: {
      type: Function,
      required: true,
    },
    selectTask: {
      type: Function,
      required: true,
    },
    deleteTask: {
      type: Function,
      required: true,
    },
  },
};
</script>


<style>
.headerLogo,
.menuToggle {
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease-in-out;
}


.headerLogo.fade-in,
.menuToggle.fade-in {
  visibility: hidden;
  opacity: 0;
  display: none;
  transition: opacity 0.3s ease-in-out;
}

.sidebar {
  height: 100%;
  width: 310px;
  background: var(--bg-color);
  padding: 15px 10px;
  border-radius: 12px;
  box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);
  transition: 0.4s;
}


.taskTitle {
  flex: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  margin-right: 0.5rem;
}

.sidebarLi {
  display: flex;
  justify-content: space-between;

  background: var(--item-color);
  list-style-type: none;

  border-radius: 12px;
  max-width: 310px;
  padding: 12px;
  width: 100%;
}

.tasksContainer {
  width: 100%;
  height: 100%;
  flex: 1;
  overflow-y: auto;
}

.sidebarHeader {
  position: relative;
  padding: 25px 20px;
  right: 0;
  top: 0;
}

.toggleCrossButton {
  display: flex;
  position: absolute;
  align-items: center;
  padding: 10px 10px;
  border: none;
  background: none;

  top: 0;
  right: 0;
}

.toggleCross {
  width: 20px;
  height: 20px;
}

.menuToggle {
  /* height: 35px; */
  width: 30px;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  transition: 0.4s ease;
}

.sidebarUl {
  color: var(--white-t-color);
  display: flex;
  flex-direction: column;
  gap: 10px;
  white-space: nowrap;
  border-radius: 8px;
  padding: 28px 15px;
  align-items: center;
  text-decoration: none;
  transition: 0.4s ease;
}

.createButton {
  background: var(--button-color);
  color: var(--white-t-color);

  position: fixed;

  max-width: 70px;
  width: 100%;

  bottom: 20px;
  border-radius: 12px;
  margin: 12px 15px;
  padding: 13px;
  transition: 0.4s ease;

}

.createButton:active {
  background: var(--button-h-color);
}

.deleteButton {
  background: var(--delete-button);

  background: none;
  border: none;
}
</style>
