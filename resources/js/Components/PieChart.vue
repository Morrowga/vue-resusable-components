<script setup>
import { Chart as ChartJS, Tooltip, Legend, ArcElement } from 'chart.js';

ChartJS.register(ArcElement, Tooltip, Legend)

const props = defineProps({
    chartData: {
        type: Array,
        required: true
    }
})

const chartData = {
  labels: ['Pie 1', 'Pie 2', 'Pie 3', 'Pie 4'],
  datasets: [{
    backgroundColor: ['#41B883', '#E46651', '#00D8FF', '#DD1B16'],
    data: props.chartData
  }],
};

const chartOptions = {
    responsive: true,
};
</script>

<template>
    <div class="pie-area">
        <Pie id="pie-chart" class="my-5" :options="chartOptions" :data="chartData"></Pie>
    </div>
</template>


<style scoped>
    .pie-area
    {
        width: 50%;
        height: 50%;
    }
</style>
