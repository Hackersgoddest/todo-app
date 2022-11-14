<template>
  <div class="main-container">
  <div class="inner-container">
    <HeaderView />
    <InputView :task="task" :showError="showError" :errorMessage="errorMessage" @addTask="addTask" />
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
const errorMessage = ref('')

watch(
  () => tasks.value.length,
  (num) => {
    if(num > 0) showClass.value = true;
    else showClass.value = false;
  },
  {immediate: true}
  );

function addTask(data) {
  if(tasks.value.indexOf(data) !== -1 || data === '') {
    showError.value = true;
    setTimeout(() => showError.value = false, 4000)
    if(data === '') errorMessage.value = 'Hey, enter a task';
    else errorMessage.value = 'Hey, the task already exist';
  }
  else {
    tasks.value.push(data);
  }
  task.value = '';
}

function deleteTask(data) {
  tasks.value = tasks.value.filter((index) => tasks.value.indexOf(index) != data);
}

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