<script setup>
import {ref, defineProps } from 'vue';

const props = defineProps({
    columns: {
        type: Object,
        required: true
    },
    rows: {
        type: Object,
        required: true
    },
    paginationOptions: {
        type: Object,
        required: true
    },
    clientSideSearch: {
        type: Object,
        required: false
    }
})

</script>
<template>
    <vue-good-table
    :columns="props.columns"
    :pagination-options="props.paginationOptions"
    :search-options="props.clientSideSearch"
    :rows="props.rows" />
</template>
