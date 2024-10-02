<script setup>
import { ref, watchEffect } from 'vue';
import { v4 as uuidv4 } from 'uuid'; 
import axios from 'axios'
import draggable from 'vuedraggable';

// Reactive states for tasks in each column
const todoTasks = ref([]);
const inProgressTasks = ref([]);
const completedTasks = ref([]);

// Function to add a new task
const addTask = () => {
  const taskName = prompt("Create a task");
  if (taskName) {
    todoTasks.value.push({ id: uuidv4(), todo: taskName });
  }
};

// Function for fetching data from api
const fetchRandomToDos = async() => {
  axios.get('https://dummyjson.com/todos?limit=10')
  .then(response => {
    todoTasks.value=(response.data.todos);
    console.log('todoresponse '+todoTasks.value.length)
  })
  
  .catch(e => {console.log(e)});
  
}

watchEffect(fetchRandomToDos)

// Function to handle dragging tasks between columns
const handleDrop = (targetColumn, task) => {
  if (targetColumn === 'todo') {
    todoTasks.value=(task);
  } else if (targetColumn === 'in-progress') {
    inProgressTasks.value=(task);
  } else if (targetColumn === 'completed') {
    completedTasks.value=(task);
  }
};
</script>

<template>
  <div class="container mx-auto">
    <div class="flex flex-row justify-between space-x-3 p-6  w-[65vw]">
      
      <!-- To-Do -->
      <div 
        class="w-1/3 bg-pink-100 h-full p-6 shadow-xl text-center"
      >
      <div class="container">
        <h3 class="text-pink-600 font-bold">To-Do </h3>
        <button @click="addTask" class="mt-4 p-2 bg-transparent text-pink-500 border-2 border-dotted border-pink-500 rounded-3xl">Create Task</button>
      </div>
        
        <div class="mt-4">

          <draggable v-model="todoTasks" tag="ul" group="tasks">
    <template #item="{ element: task }">
      <div class="bg-white p-4 shadow-md mb-2 cursor-pointer">{{ task.todo }}</div>
    </template>
  </draggable>
        </div>
        
      </div>

      <!-- In-Progress -->
      <div 
        class="w-1/3 bg-purple-100 h-full p-6 shadow-xl text-center"
      >
        <h1 class="text-purple-600">In-Progress</h1>
        
        <div class="mt-4">
          <draggable v-model="inProgressTasks" tag="ul" group="tasks">
    <template #item="{ element: task }">
      <div class="bg-white p-4 shadow-md mb-2 cursor-pointer">{{ task.todo }}</div>
    </template>
  </draggable>
        </div>
      </div>

      <!-- Completed -->
      <div 
        class="w-1/3 bg-blue-100 h-full p-6 shadow-xl text-center"
      >
        <h1 class="text-blue-600">Completed</h1>
        <div class="mt-4">
          <draggable v-model="completedTasks" tag="ul" group="tasks">
    <template #item="{ element: task }">
      <div class="bg-white p-4 shadow-md mb-2 cursor-pointer">{{ task.todo }}</div>
    </template>
  </draggable>    
        </div>
      </div>

  
    </div>
  </div>
</template>

