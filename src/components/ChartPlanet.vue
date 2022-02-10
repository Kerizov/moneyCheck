<template>
  <div>
    <canvas id="planet-chart"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js';
import planetChartData from "../chart-planet";

export default {
  name: 'ChartPlanet',
  data(){
    return{
      planetChartData: planetChartData
    }
  },
  mounted() {
    const ctx = document.getElementById('planet-chart');
    new Chart(ctx, this.planetChartData);
  }
}
</script>

<style>

</style>



<!--data: {-->
<!--labels: ["Mercury", "Venus", "Earth"],-->
<!--datasets: [-->
<!--{-->
<!--label: "Number of Moons",-->
<!--data: [5, 20],-->
<!--backgroundColor: ['rgb(255, 99, 132)',-->
<!--'rgb(54, 162, 235)',-->
<!--'rgb(255, 205, 86)'],-->
<!--borderColor: "#36495d",-->
<!--borderWidth: 0,-->
<!--hoverOffset: 4,-->
<!--}-->
<!--]-->
<!--},-->
<!--options: {-->
<!--responsive: true,-->
<!--cutoutPercentage: 80,-->
<!--}-->