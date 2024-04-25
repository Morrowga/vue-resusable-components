<script setup>
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';
ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

const props = defineProps({
    chartData: {
        type: Array,
        required: true
    }
})

const chartData = {
  labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'Augest', 'September', 'October', 'November', 'December' ],
  datasets: [{
    label: 'Bar Chart',
    data: props.chartData
  }]
};

const chartOptions = {
  responsive: true,
  legend: {
    display: false
  }
};
</script>

<template>
    <Bar id="bar-chart" :options="chartOptions" :data="chartData"></Bar>
</template>
