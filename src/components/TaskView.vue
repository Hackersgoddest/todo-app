<template>
  <template class="main-task-container">
    <p v-if="!tasks.length">No Task Added Yet</p>
    <ul v-else class="task-container">
      <li v-for="(task, index) in tasks" :key="task" class="task">
        <div class="task-detail">
          {{ task.title }}
        </div>
        <div class="btn-container">
          <button @click="$emit('editTask', task.id, index)">edit</button>
          <button @click="$emit('deleteTask', task.id, index)">delete</button>
        </div>
      </li>
    </ul>
    <p class="author">By frank oppong konadu</p>
  </template>
</template>

<script setup>
const props = defineProps(["tasks"]);
</script>

<style scoped>
.main-task-container {
  height: 50%;
  width: 100%;
  display: flex;
  flex-direction: column;
  background-color: white;
}

.task-container {
  display: flex;
  flex-direction: column;
  justify-content: start;
  width: 100%;
  /* padding: 0; */
  height: 95%;
  overflow: scroll;
  background-color: white;
}

.task-container.center {
  display: flex;
  justify-content: center;
}

.task {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 5px;
  overflow: auto;
  background-color: khaki;
  min-height: 70px;
  position: relative;
  border-radius: 5px;
}

.task-detail {
  font-size: 20px;
  font-family: "montserrat", sans-serif;
  height: 35px;
  display: flex;
  align-items: flex-start;
  margin-left: 5px;
  color: rgb(68 67 67 / 70%);
}

.btn-container {
  /* display: none; */
  display: flex;
  position: sticky;
  top: 17px;
  right: 5px;
}

.btn-container button {
  height: 30px;
  border-radius: 50px;
  border: none;
  color: rgba(255, 255, 255, 0.5);
  font-weight: 500;
}

.btn-container :first-child {
  width: 50px;
  margin-right: 5px;
  background-color: rgba(218, 165, 32, 0.3);
}

.btn-container :last-child {
  background-color: rgba(255, 0, 0, 0.3);
  width: 47px;
}

.btn-container :first-child:hover {
  background-color: rgba(218, 165, 32, 1);
  color: white;
  transition: background-color 0.5s;
}

.btn-container :last-child:hover {
  background-color: rgba(255, 0, 0, 1);
  color: white;
  transition: background-color 0.5s;
}

p {
  height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 25px;
  color: #777;
}

.author {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  font-family: sans-serif;
  font-size: 20px;
  font-weight: 500;
  height: 25px;
  color: darkgoldenrod;
}
</style>
