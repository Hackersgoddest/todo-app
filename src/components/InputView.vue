<template>
    <form
      @submit.prevent="$emit('addTask', task.title.toLowerCase())"
      class="input-container"
    >
       <div
         class="textarea"
       >
            <input 
            @keydown="$emit('hideError')"
              v-model.trim="task.title"
              id="task"
              name="task"
              placeholder="Enter a task"
              type="text"
            >
            <!-- {{task}} -->
        </div>
        <div class="button">
            <button>ADD</button>
        </div>
        <div
          class="error"
        >
            <span v-if="isError">{{ errorMessage }}</span>
        </div>
    </form>

</template>

<script setup>
const props = defineProps(['task', 'errorMessage', 'isError'])
</script>

<style scoped>
.input-container {
    position: relative;
    padding: 0 5px;
    display: flex;
    width: 100%;
    /* justify-content: space-between; */
    align-items: center;
    height: 20%;
    /* margin-bottom: 30px; */
}

.input-container>div {
    height: 40%;
}

.textarea {
    /* flex: 1; */
    width: 80%;
}

input {
    width: 100%;
    height: 100%;
    text-align: center;
    font-size: 20px;
    border-radius: 5px;
    border: none;
    color: #7a7a7a;
}

input:focus {
    outline: none;
}

.error {
    position: absolute;
    left: 5px;
    bottom: -40px;
    color: red;
    display: flex;
    justify-content: start;
    /* align-items: center; */
    /* padding-left: 5px; */
}

.button {
    width: 20%;
    text-align: end;
}

button {
    height: 100%;
    background-color: orange;
    border: none;
    color: white;
    font-weight: 700;
    width: 65%;
    border-radius: 5px;
}

button:hover {
    background-color: green;
}

button:focus {
    outline: none;
}

@media screen and (min-width: 450px) {
    button {
        width: 50%;
    }
}
</style>