<script setup>

import { ref, computed } from 'vue';
import Task from './Task.vue';


const newTask = ref('')
const id = ref(1)
const tasks = ref([])
const buttons = ['Active tasks', 'Done tasks', 'All tasks',]
const activeButton = ref(buttons[0])
let isLoading = ref(true)


const fetchData = async () => {                       
  fetch('https://jsonplaceholder.typicode.com/users/1/todos')
    .then((response) => response.json())
    .then((json) => {
      tasks.value = json
      console.log(tasks.value);
    })
    .finally(() => {
      isLoading.value = false
      console.log(isLoading.value);
    })
}

document.addEventListener('DOMContentLoaded', fetchData)

const handleTasks = computed( () => {     
  if (!isLoading.value) {
    switch (activeButton.value) {
      case 'Active tasks':
        return tasks.value.filter((task) => !task.completed)
      case 'Done tasks':
        return tasks.value.filter((task) => task.completed)
      case 'All tasks':
        return tasks.value
      default:
        console.log('Something goes wrong')
        break;
    }
  }                     
})

function addTask () {
  if (newTask.value) {
    id.value = tasks.value[tasks.value.length - 1].id
    tasks.value.push({userId: 1, id: ++id.value, title: newTask.value, completed: false})
    newTask.value = ''
    setActiveButton('Active tasks')
    // console.log(tasks.value);
  }
}
function deleteTask (task) {
  tasks.value = tasks.value.filter((t) => t !== task)
  console.log(tasks.value);
}
function checkTask (task) {
  tasks.value.forEach((t) => {
    t === task ? task.completed = true : false
  })
}
function setActiveButton (button) {
  if (activeButton.value) {
    activeButton.value = null
  }
  activeButton.value = button
}

</script>

<template>

  <div class="main">
    <div class="todo-wrapper">
      
      <nav class="nav">
        <button
        v-for="(button, index) in buttons"
        :key="index"
        :class="{active : activeButton === button}"
        @click="setActiveButton(button);"
        >
        {{ button }}</button>
      </nav>
      
      <div class="new-task">
        <form @submit.prevent="addTask" class="form-task">
          <input v-model="newTask" type="text" class="input-task" placeholder="New task here..">
          <img @click="addTask" src="../assets/icons/add.svg" alt="" class="add-task">
        </form>
      </div>
      
      <div class="tasks">

        <Task 
        v-for="task in handleTasks"
        :key="task.id"
        :taskText="task.title"
        @delete-task="deleteTask(task)"
        @check-task="checkTask(task)"
        :show-check-btn="!task.completed"
        />

      </div>

    </div>
  </div>

</template>

<style scoped>

img {
  width: 2.4rem;
  height: 2.4rem;
  cursor: pointer;
}
.main {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}

.title {
  padding-top: 4rem;
  margin-bottom: 1rem;
} 

.todo-wrapper {
  width: 90rem;
  height: 60rem;
  background-color: rgb(240 238 238);
  display: flex;
  flex-direction: column;
  overflow: auto;
}

.nav {
  width: 50rem;
  min-height: 5rem;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  gap: 5rem;
  align-self: center;
  border-bottom: 0.2rem solid #000;
}

.nav button {
  width: 10rem;
  height: 3rem;
  background-color: #fff;
  border: none;
  cursor: pointer;
}

.tasks {
  padding-top: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  /* overflow: auto; */
}

.new-task {
  display: flex;
  justify-content: center;
  padding: 1.5rem 0;
  border-bottom: 0.2rem solid #000;
  align-self: center;
}

.form-task {
  display: flex;
  justify-content: space-between;
  width: 50rem;
  padding: 1.2rem 0.8rem;
  background-color: #fff;
}

.input-task {
  width: 90%;
  border: none;
  outline: none;
}

.active  {
  color: #fff;
  background-color: #000 !important;
}




</style>
