<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';

//Hiding the dropdown
const isDropdownVisible = ref(false);

const toggleDropdown = () => {
  isDropdownVisible.value = !isDropdownVisible.value;
};

const handleClickOutside = (event) => {
  const dropdown = document.getElementById('dropdown-menu');
  const button = document.getElementById('dropdown-button');
  
  if (dropdown && !dropdown.contains(event.target) && button && !button.contains(event.target)) {
    isDropdownVisible.value = false;
  }
};

const search = () => {
    todotask
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside);
});

onBeforeUnmount(() => {
  document.removeEventListener('click', handleClickOutside);
});
</script>

<template>
  <nav class="bg-yellow-50 shadow-md">
    <div class="container mx-auto flex justify-between items-center py-4 px-6">
      <!-- Logo or Title -->
      <div class="text-xl font-bold text-gray-700">P's Tasks</div>
      
      <!-- Search Bar -->
      <div class="flex items-center space-x-4">
        <input
          type="text"
          placeholder="Search for task"
          class="border border-gray-300 rounded-md px-3 py-2 focus:outline-none focus:ring-2 focus:ring-yellow-300"
        />
        
        <button
          @click="search"
          id="dropdown-button"
          class="bg-yellow-500 text-white px-4 py-2 rounded-md focus:outline-none hover:bg-yellow-600"
        >
         Search
        </button>
        <button
          @click="toggleDropdown"
          id="dropdown-button"
          class="bg-yellow-500 text-white px-4 py-2 rounded-md focus:outline-none hover:bg-yellow-600"
        >
          Menu
        </button>

        <!-- Dropdown Menu -->
        <ul
          v-if="isDropdownVisible"
          id="dropdown-menu"
          class="absolute bg-white border border-gray-300 mt-12 rounded-md shadow-lg w-48 py-2 right-6"
        >
          <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer">To-Do</li>
          <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer">In-progress</li>
          <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer">Completed</li>
        </ul>
      </div>
    </div>
  </nav>
</template>
