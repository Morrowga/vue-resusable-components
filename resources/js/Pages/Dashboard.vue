<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import {ref, defineProps, onMounted,watch } from 'vue';
import axios from 'axios';
import ClientSideTable from '@/Components/ClientSideTable.vue';
import ServerSideTable from '@/Components/ServerSideTable.vue';
import BarChart from '@/Components/BarChart.vue';
import LineChart from '@/Components/LineChart.vue';
import PieChart from '@/Components/PieChart.vue';
import BubbleChart from '@/Components/BubbleChart.vue';
import MultipleFileInput from '@/Components/MultipleFileInput.vue';

const props = defineProps(['users']);

const date = ref();

const clientSideSearch = ref({
    enabled: true,
    trigger: 'enter',
    placeholder: 'Search this table',
})

const clientSidePaginationOptions = ref({
        enabled: true,
        mode: 'records',
        perPage: 10,
        position: 'bottom',
        perPageDropdown: [3, 7, 9],
        dropdownAllowAll: false,
        setCurrentPage: 2,
        nextLabel: 'next',
        prevLabel: 'prev',
        rowsPerPageLabel: 'Rows per page',
        ofLabel: 'of',
        pageLabel: 'page',
        allLabel: 'All',
})

const serverSidePaginationOptions = {
  enabled: true,
};

const endpoint_url = ref('api/users')

const columns = ref([
    {
        label: 'Name',
        field: 'name',
    },
    {
        label: 'Email',
        field: 'email',
    },
    {
        label: 'Created At',
        field: 'created_at',
    },
])

const formattedDate = () => {
    const currentDate = new Date();
    const year = currentDate.getFullYear();
    const month = String(currentDate.getMonth() + 1).padStart(2, '0'); // Month is zero-based, so we add 1
    const day = String(currentDate.getDate()).padStart(2, '0');

    const formattedDate = `${year}-${month}-${day}`;

    date.value = formattedDate;
    console.log(date.value);
}

watch(date.value, (newValue, oldValue) => {
  console.log('Date changed:', newValue);
});

onMounted(() => {
    formattedDate()
})
</script>

<template>
    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Dashboard</h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <h1 class="font-bold text-xl">Client Side Table</h1>
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg my-5">
                    <ClientSideTable :columns="columns" :paginationOptions="clientSidePaginationOptions" :rows="props.users" :clientSideSearch="clientSideSearch" />

                </div>

                <h1 class="font-bold text-xl mt-5">Server Side Table</h1>
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg my-5">
                    <ServerSideTable :columns="columns" :paginationOptions="serverSidePaginationOptions" :endPoint="endpoint_url" />
                </div>

                <h1 class="font-bold text-xl mt-5">Date Picker</h1>
                <VueDatePicker v-model="date" class="my-5"></VueDatePicker>

                <h1 class="font-bold text-xl mt-5">Bar Chart</h1>
                <BarChart :chartData="[40,50,80]" />
                <h1 class="font-bold text-xl mt-5">Line Chart</h1>
                <LineChart :chartData="[10,15,18,25,30,35,40,50,58,80,85,90,95]" />
                <h1 class="font-bold text-xl mt-5">Pie Chart</h1>
                <PieChart :chartData="[10,15,18,25]" />
                <h1 class="font-bold text-xl mt-5">Bubble Chart</h1>
                <BubbleChart :chartData="[
                        {
                        x: 20,
                        y: 25,
                        r: 5
                        },
                        {
                        x: 40,
                        y: 10,
                        r: 10
                        },
                        {
                        x: 30,
                        y: 22,
                        r: 30
                        }
                    ]"
                />
                <h1>Multifile File Upload</h1>
                <MultipleFileInput class="my-5" />
            </div>
        </div>
    </AuthenticatedLayout>
</template>
