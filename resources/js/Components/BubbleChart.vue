<script setup>
import { Chart as ChartJS, Tooltip, Legend, PointElement, LinearScale } from 'chart.js';

ChartJS.register(LinearScale, PointElement, Tooltip, Legend)

const props = defineProps({
    chartData: {
        type: Array,
        required: true
    }
})

const chartData = {
  labels: ['Pie 1', 'Pie 2', 'Pie 3', 'Pie 4'],
  datasets: [{
    label: 'Bubble Chart',
    backgroundColor: '#f87979',
    data: props.chartData
  }],
};

const chartOptions = {
    responsive: true,
};
</script>

<template>
    <Bubble id="bubble-chart" class="my-5" :options="chartOptions" :data="chartData"></Bubble>
</template>
