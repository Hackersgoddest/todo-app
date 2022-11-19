<template>
  <div class="main-container">
    <div class="inner-container">
      <HeaderView />
      <InputView :task="task" :showError="showError" :isSpace="isSpace" :isClear="isClear" @addTask="addTask" />
      <TaskView :tasks="tasks" :showClass="showClass" @deleteTask="deleteTask" @editTask="editTask" />
    </div>
  </div>
</template>

<script setup>
import HeaderView from "./components/HeaderView.vue"
import InputView from "./components/InputView.vue"
import TaskView from "./components/TaskView.vue"
import { ref, watch } from "vue";
let task = ref('');
let tasks = ref([]);
let showError = ref(false);
let showClass = ref(false);
let isSpace = ref(false);
let isClear = ref(true);
let isEdit = ref(false);
let index = ref(0);

// Checks whether a task has been added or not using the lenght property of the array holding the task.
watch(
  () => tasks.value.length,
  (num) => {
    if (num > 0) showClass.value = true;
    else showClass.value = false;
  },
  { immediate: true }
);

// clears the input fields
function clearInput() {
  isClear.value = false;
  setTimeout(() => isClear.value = true, 0);

}

// add task to the the array variable holding the tasks.
function addTask(data) {
  if (tasks.value.indexOf(data) !== -1 || data === '') {
    showError.value = true;
    if (data == '') {
      isSpace.value = true
      task.value = '';
    }
    else {
      task.value = data
      data = '';
      isSpace.value = false
    }
    setTimeout(() => showError.value = false, 1000);
  }
  else {
    if (isEdit.value) {
      tasks.value[index.value] = data;
    }
    else tasks.value.push(data);
    clearInput();
    task.value = '';
    index.value = 0;
    isEdit.value = false;
  }
}

// delete task from the array variable holding the tasks
function deleteTask(data) {
  tasks.value = tasks.value.filter((index) => tasks.value.indexOf(index) != data);
}

// edit task in the array variable holding the task
function editTask(data) {
  task.value = tasks.value[data];
  isEdit.value = true;
  index.value = data;
  // tasks.value = tasks.value.filter((index) => tasks.value.indexOf(index) != data);
}


</script>

<style scoped>
.main-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: black;
}

.inner-container {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  height: 90%;
  width: 95%;
  background-color: gold;
  border: none;
  border-radius: 10px;
  /* padding-bottom: 15px; */
  overflow: hidden;
}

@media screen and (min-width: 501px) {
  .inner-container {
    max-width: 700px;
  }

  .header-container {
    height: 100px;
  }
}
</style>