<script setup>
import {ref, defineProps, onMounted } from 'vue';

const props = defineProps({
    columns: {
        type: Object,
        required: true
    },
    paginationOptions: {
        type: Object,
        required: true
    },
    endPoint: {
        type: String,
        required: true
    }
})

const totalRows = ref(0);
const rows = ref([]);
const to = ref(0);
const lastPage = ref(0);
const sortWith = ref('Sorting');
const from = ref(0);
const serverPerPage = ref(0);
const currentPage = ref(1);
const searchQuery = ref('');

const fetchData = async (page = 1) => {
  try {
    const response = await axios.get( props.endPoint + '?page=' + currentPage.value + '&search=' + searchQuery.value + '&sort=' + sortWith.value);
    totalRows.value = response.data.total;
    rows.value = response.data.data.data;
    lastPage.value = response.data.data.last_page
    to.value = response.data.data.to;
    from.value = response.data.data.from;
    serverPerPage.value = response.data.data.per_page;

    console.log(lastPage.value);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
    fetchData(currentPage.value);
  }
};

const nextPage = () => {
  if (currentPage.value < lastPage.value) {
    currentPage.value++;
    fetchData(currentPage.value);
  }
};

const sort = () => {
    fetchData();
}

const onSearch = () => {
    fetchData(currentPage.value = 1);
}

onMounted(() => {
  fetchData();
});
</script>
<template>
<div class="flex justify-end">
    <select @change="sort" v-model="sortWith"  class="px-5 py-2 my-4 border border-gray-300 text-dark rounded-md focus:outline-none focus:border-blue-500">
        <option>Sorting</option>
        <option v-for="(column,index) in props.columns" :key="index" :value="column.field">{{  column.label  }}</option>
    </select>
    <input v-model="searchQuery" @keyup="onSearch" type="text" placeholder="Search..." class="px-3 py-2 my-4 mx-5 border border-gray-300 rounded-md focus:outline-none focus:border-blue-500">
</div>
<vue-good-table
    class="role-data-table"
    mode="remote"
    :totalRows="totalRows"
    styleClass="vgt-table"
    :pagination-options="props.paginationOptions"
    :rows="rows"
    :sort-options="{
        enabled: false,
    }"
    :columns="props.columns"
>
    <template #pagination-bottom>
        <div class="pa-4 px-5 py-5">
            <div
                cols="12"
                class="d-flex justify-space-between"
            >
                <span
                    >Showing {{ from }} to
                    {{ to }} of
                    {{ totalRows }} entries</span
                >
                <div>
                    <div class="flex justify-between my-5">
                        <div>
                            <span class="me-2">Show</span>
                            <select v-model="serverPerPage" class="px-2 py-1 border border-gray-300 rounded">
                                <option v-for="option in [10,20,50,100]" :key="option" :value="option">{{ option }}</option>
                            </select>
                        </div>

                        <nav class="flex items-center justify-end">
                        <ul class="pagination">
                            <li>
                            <button @click="prevPage" :disabled="currentPage === 1" :class="{ 'disabled': currentPage === 1 }" class="px-2 py-1 border border-gray-300 rounded cursor-pointer">Previous</button>
                            </li>
                            <li>
                            <button @click="nextPage" :disabled="currentPage === lastPage" :class="{ 'disabled': currentPage === lastPage }" class="px-2 py-1 border border-gray-300 rounded cursor-pointer">Next</button>
                            </li>
                        </ul>
                        </nav>
                    </div>
                </div>
            </div>
        </div>
    </template>
</vue-good-table>

</template>

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
