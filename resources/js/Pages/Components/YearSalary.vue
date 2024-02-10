<template>
    <div class="bg-white shadow-md border border-gray-200 py-4 px-4 rounded-lg">
        <h3 class="pb-3">Year</h3>
        <LineChart :chartData="chartData" :options="options" />
    </div>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue';
import { LineChart } from 'vue-chart-3'; 
import { Chart, registerables } from "chart.js";
import axios from 'axios';

Chart.register(...registerables);

export default defineComponent({
    name: 'YearChart', // Cambiar el nombre del componente
    components: { LineChart }, 
    setup() {
        const chartData = ref({});

        const fetchData = async () => {
            try {
                const response = await axios.get('https://48fk51szmi.execute-api.us-east-1.amazonaws.com/dev/salaries/year');
                const data = JSON.parse(response.data.data);
                const labels = data.map(item => item.work_year);
                const values = data.map(item => item.average_salary);

                chartData.value = {
                    labels: labels,
                    datasets: [
                        {
                            label: 'Average Salary', // Etiqueta del conjunto de datos
                            data: values,
                            borderColor: 'blue', // Color de la línea
                            fill: false // No rellenar debajo de la línea
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
