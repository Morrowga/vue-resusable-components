<script setup>
import { Chart as ChartJS, Title, Tooltip, Legend, CategoryScale, LinearScale, LineElement, PointElement } from 'chart.js';
ChartJS.register(Title, Tooltip, Legend, PointElement, LineElement, CategoryScale, LinearScale);

const props = defineProps({
    chartData: {
        type: Array,
        required: true
    }
})

const chartData = {
  labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'Augest', 'September', 'October', 'November', 'December' ],
  datasets: [{
    label: 'Line Chart',
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
    <Line id="line-chart" :options="chartOptions" :data="chartData"></Line>
</template>
