<template>
    <div> 

        <h2>Dashboard</h2>

        <div class="chart-container">
            <BarChart :chart-data="dataChart"/>
            <LineChart :chart-data="dataEvolution"  />
            <LineChart :chart-data="chartDataExpensive"/>
        </div>

    </div>

</template>

<script setup>
import { computed } from 'vue';
import { Chart as ChartJs, registerables } from 'chart.js';
import { Bar as BarChart, Line as LineChart } from 'vue-chartjs';
ChartJs.register(...registerables);

const props = defineProps(["subscriptions"]);


const dataChart = computed(() => {
    const group = [];
    props.subscriptions.forEach(sub => {
        group[sub.Tipo] = (group[sub.Tipo] || 0 ) + parseFloat(sub.Precio);
    });
    return { labels: Object.keys(group), datasets: [{ data: Object.values(group), label: 'Gastos por tipo de suscripción' }] }

});


const dataEvolution = computed(() => {
    const mountly = {};
    props.subscriptions.forEach(sub => {
        const month = new Date(sub.FechaInicio).toISOString().slice(0,7);
        mountly[month] = (mountly[month] || 0) + parseFloat(sub.Precio);
    });
    return { labels: Object.keys(mountly), datasets: [{ data: Object.values(mountly), label: 'Evolución' }] }
});

const mostExpensive = computed(() => {
    return [...props.subscriptions]
    .sort((a,b) => b.Precio - a.Precio)
    .slice(0,5);
});


const chartDataExpensive = computed(()=> {
    return {
        labels: mostExpensive.value.map((sub) => sub.Nombre),
        datasets: [
            {
                label: "Precio por suscripción",
                data: mostExpensive.value.map((sub) => sub.Precio),
                backgroundColor: ["#42A5F5", "#66BB6A"]
            },
        ],
    };
});


</script>

<style lang="scss" scoped>


h2 {
    margin: 2rem 0rem;
    font-size: 3rem;
}

.chart-container {
    display: flex;
    margin: 3rem 0rem;
}

canvas {
    max-width: 50%;
    width: 100%;
    height: 30rem;
}

</style>