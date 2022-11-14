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
const task = ref('');
const tasks = ref([]);
const showError = ref(false);
const showClass = ref(false);
const isSpace = ref(false);
const isClear = ref(true);

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
    setTimeout(() => isClear.value = true, 0)
}

// add task to the the array variable holding the tasks.
function addTask(data) {
  if (tasks.value.indexOf(data) !== -1 || data === '') {
    showError.value = true;
    setTimeout(() => showError.value = false, 3000)
    if (data == '') isSpace.value = true
    else {
      task.value = data
      isSpace.value = false
    }
  }
  else { 
    tasks.value.push(data);
    clearInput();
    task.value = '';
  }
}

// delete task from the array variable holding the tasks
function deleteTask(data) {
  tasks.value = tasks.value.filter((index) => tasks.value.indexOf(index) != data);
}

// edit task in the array variable holding the task
function editTask(data) {
  task.value = tasks.value[data];
  tasks.value = tasks.value.filter((index) => tasks.value.indexOf(index) != data);
}


</script>

<style scoped>
.main-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: 'Lucida Sans';
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