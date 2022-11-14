<template>
    <ul class="task-container" :class="!showClass ? 'center' : ''">
        <p v-show="!showClass">
            No Task Added Yet
        </p>
        <template v-for="(task, index) in tasks" :key="task">
            <li :class="showClass ? 'task' : ''">
                <div :class="showClass ? 'task-detail' : ''">
                    {{ task }}
                </div>
                <div :class="showClass ? 'btn-container' : ''">
                    <button @click="$emit('editTask', index)">edit</button>
                    <button @click="$emit('deleteTask', index)">delete</button>
                </div>
            </li>
        </template>
    </ul>
</template>

<script setup>
const props = defineProps(['tasks', 'showClass']);
</script>

<style scoped>
.task-container {
    display: flex;
    flex-direction: column;
    justify-content: start;
    width: 100%;
    padding: 0;
    height: 50%;
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
    height: 35px;
    display: flex;
    align-items: flex-start;
    margin-left: 5px;
    color: rgba(0, 0, 0, 0.7);
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
</style>