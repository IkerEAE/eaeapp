<template>
    <div class="bg-white shadow-md border border-gray-200 py-4 px-4 rounded-lg">
        <h3 class="pb-3">Position</h3>
        <DoughnutChart :chartData="chartData" :options="options" />
    </div>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue';
import { DoughnutChart } from 'vue-chart-3'; // Importa DoughnutChart en lugar de PieChart
import { Chart, registerables } from "chart.js";
import axios from 'axios';

Chart.register(...registerables);

export default defineComponent({
    name: 'Position',
    components: { DoughnutChart }, // Usa DoughnutChart en lugar de PieChart
    setup() {
        const chartData = ref({});

        const fetchData = async () => {
            try {
                const response = await axios.get('https://48fk51szmi.execute-api.us-east-1.amazonaws.com/dev/salaries/position');
                const data = JSON.parse(response.data.data);
                const labels = data.map(item => item.job_title);
                const values = data.map(item => item.average_salary);

                chartData.value = {
                    labels: labels,
                    datasets: [
                        {
                            data: values,
                            backgroundColor: ['#77CEFF', '#0079AF', '#123E6B']
                        },
                    ],
                }

            } catch (error) {
                console.log(error);
            }
        }

        onMounted(() => {
            fetchData();
        });

        const options = {
            plugins: {
                legend: {
                    display: true
                }
            }
        }

        return { chartData, options };
    },
});
</script>
