<template>
    <nav class="flex items-center justify-center">
      <ul class="pagination">
        <li>
          <button @click="prevPage" :disabled="serverPage === 1" :class="{ 'disabled': serverPage === 1 }" class="px-2 py-1 border border-gray-300 rounded cursor-pointer">Previous</button>
        </li>
        <li v-for="pageNumber in totalVisiblePages" :key="pageNumber" :class="{ 'active': serverPage === pageNumber }">
          <button @click="goToPage(pageNumber)" :class="{ 'font-bold': serverPage === pageNumber }" class="px-2 py-1 border border-gray-300 rounded cursor-pointer">{{ pageNumber }}</button>
        </li>
        <li>
          <button @click="nextPage" :disabled="serverPage === props.roles.meta.last_page" :class="{ 'disabled': serverPage === props.roles.meta.last_page }" class="px-2 py-1 border border-gray-300 rounded cursor-pointer">Next</button>
        </li>
      </ul>
    </nav>
  </template>

  <script setup>
  import { ref } from 'vue';

  const serverPage = ref(1);
  const totalVisiblePages = 5; // Adjust as needed

  const prevPage = () => {
    if (serverPage.value > 1) {
      serverPage.value--;
      onPageChange();
    }
  };

  const nextPage = () => {
    if (serverPage.value < props.roles.meta.last_page) {
      serverPage.value++;
      onPageChange();
    }
  };

  const goToPage = (pageNumber) => {
    serverPage.value = pageNumber;
    onPageChange();
  };

  const onPageChange = () => {
    console.log('Page changed:', serverPage.value);
  };
  </script>

  <style scoped>
  .pagination {
    display: flex;
    list-style: none;
  }

  .pagination li {
    margin: 0 2px;
  }

  .pagination li button {
    background-color: transparent;
  }

  .pagination li button:hover {
    background-color: #e2e8f0;
  }

  .pagination li.active button {
    background-color: #4299e1;
    color: white;
  }

  .pagination li.disabled button {
    cursor: not-allowed;
    opacity: 0.5;
  }
  </style>
