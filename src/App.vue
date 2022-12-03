<template>
  <div class="main-container">
    <div class="inner-container">
      <HeaderView />
      <InputView :isError="isError" :error-message="errorMessage" :task="newTaskItem" @addTask="addTask" />
      <TaskView :tasks="tasks" @deleteTask="deleteTask" @editTask="editTask" />
    </div>
  </div>
</template>

<script setup>
import axios from "axios"
import HeaderView from "./components/HeaderView.vue"
import InputView from "./components/InputView.vue"
import TaskView from "./components/TaskView.vue"
import { ref, onMounted } from "vue";
let newTaskItem = ref({ title: '' });
let tasks = ref([]);
const newTaskArray = ref([]);
let errorMessage = ref('');
let isError = ref(false);
let isEdit = ref(false);
let currentTaskIndex = ref(0);

const loadData = async () => {
  try {
    let response = await axios.get('https://jsonplaceholder.typicode.com/todos')
    // tasks.value = response.data.map(element => element.title);
    tasks.value = response.data;
  } catch (err) {
    alert(err.message)
  }
}


onMounted(() => loadData())

// add task to the the array variable holding the tasks.
function addTask(newTask) {
  newTaskArray.value = tasks.value.map(task => task.title)
  if (newTask == '') {
    isError.value = true;
    errorMessage.value = "hey, enter a task";
    return;
  }
  else if (newTaskArray.value.indexOf(newTask) != -1) {
    isError.value = true;
    errorMessage.value = "hey, task already exist";
    return;
  }
  else {
    if (isEdit.value) {
      axios.put('https://jsonplaceholder.typicode.com/todos/' + (currentTaskIndex.value + 1),
        {
          id: currentTaskIndex.value + 1,
          // id: 1,
          title: newTask,
          body: 'bar',
          // userId: 1,
          userId: currentTaskIndex.value + 1,
        }).then(res => {
          tasks.value.splice((res.data.id - 1), 1, res.data)
        }).catch(err => alert(err.message))
      isError.value = false;
      currentTaskIndex.value = 0;

    }
    else {
      axios.post('https://jsonplaceholder.typicode.com/todos',
        {
          title: newTask,
          body: 'bar',
          userId: 1,
        }).then(res => {
          tasks.value.push(res.data)
        }).catch(err => alert(err.message))
      isError.value = false;
    }
  }
  newTaskItem.value = { title: '' };
  isEdit.value = false;
}

// delete task from the array variable holding the tasks
function deleteTask(taskIndex) {
  axios.delete('https://jsonplaceholder.typicode.com/todos/' + (taskIndex + 1))
    .then(res => {
      tasks.value.splice((res.data.id - 1), 1)
    })
    .catch(err => alert(err.message))
}

// edit task in the array variable holding the task
const editTask = async (taskIndex) => {
  try {
    let response = await axios.get('https://jsonplaceholder.typicode.com/todos/' + (taskIndex + 1))
    // tasks.value = response.data.map(element => element.title);
    newTaskItem.value = response.data;
    isEdit.value = true;
    currentTaskIndex.value = taskIndex;
  } catch (err) {
    alert(err.message)
  }

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