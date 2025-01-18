
<template>
  <div class="TodoApp">
    <form class="todoForm" @submit.prevent="saveTask">
      <div class="blokInput">
        <input type="text" id="title" v-model="newTask.title" placeholder="Введите заголовок"
          @input="checkFormValidity" />
      </div>
      <div class="blokTextarea">
        <textarea v-model="newTask.text" id="text" placeholder="Введите описание" @input="checkFormValidity"></textarea>
      </div>
      <button class="saveButton" v-if="isFormValid" type="submit">Сохранить</button>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    selectedTask: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      newTask: { ...this.selectedTask }, 
      isFormValid: false, 
    };
  },
  watch: {
    selectedTask: {
      immediate: true,
      handler(newTask) {
        this.newTask = { ...newTask }; 
        this.isFormValid = newTask.title && newTask.text; 
      },
    },
  },
  methods: {
    checkFormValidity() {
      this.isFormValid = this.newTask.title && this.newTask.text;
    },

    saveTask() {
      if (this.isFormValid) {
        this.$emit('task-changed', { ...this.newTask });
        this.$emit('save-task', { ...this.newTask }); 

        this.newTask = { title: '', text: '' }; 
        this.isFormValid = false; 
      }
    },
  },
};
</script>


<style>
.selected {
  background-color: #1a73e8;
  color: white;
}

input,
textarea {
  max-width: 330px;
  max-height: 330px;
  height: 100%;
  width: 100%;
  border: none;
  outline: none;
  background: transparent;
  font-size: 16px;
  resize: none;
  color: #ffffff;
}


input,
textarea::placeholder {
  color: rgb(255, 255, 255);
}

.formButton {
  height: 50px;
  border-radius: 12px;
}

#title {
  height: 100%;
  width: 100%;
  border-radius: 12px;
  padding: 10px;
}

#text {
  height: 100%;
  width: 100%;
  border-radius: 12px;
  padding: 10px;

}

.blokInput {
  max-height: 50px;
  height: 100%;
}

.blokTextarea {
  height: 100%;
  flex: 1;
}

.TodoApp {
  background: var(--bg-color);
  width: 100%;
  height: 100%;
  border-radius: 12px;
}

.todoForm {
  display: flex;
  flex-direction: column;
  gap: 20px;
  width: 100%;
  height: 100%;
  padding: 26px;
}

.saveButton {
  background: var(--button-color);
  color: var(--white-t-color);

  position: fixed;
  bottom: 20px;

  width: calc(310px - 40px);

  border-radius: 12px;
  margin: 12px 15px;
  padding: 13px;
}

.saveButton:active {
  background: var(--button-h-color);
}
</style>