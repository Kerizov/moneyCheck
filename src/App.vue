<template>
  <div class="wrapper">
    <button type="button" @click="shuffleData">Shuffle</button>
    <button type="button" @click="addData">addData</button>
    <button type="button" @click="view">view</button>
    <my-dialog @create="createData"></my-dialog>
    <DoughnutChart
        class="doughnut-chart"
        :data="testData"
        :options="options"
        @chart:render="handleChartRender"
    />
  </div>

</template>

<script>
import { computed, ref } from "vue";
import { shuffle } from "lodash";
import { DoughnutChart } from "vue-chart-3";

import MyDialog from "./components/UI/MyDialog";

export default {
  name: "App",
  components: { DoughnutChart, MyDialog },
  data (){
    return{
      chartData: [],
      DialogVisible: false,
    }
  },
  methods: {
    view(){
      console.log(this.chartData[0].value);
    },
    createData(chartData){
      this.chartData.push(chartData);

    }
  },
  setup() {
    const dataValues = ref([30, 40, 60, 70, 5]);
    const labelsValues = ref(["Paris", "Nîmes", "Toulon", "Perpignan", "Autre"]);

    const testData = computed(() => ({
      labels: labelsValues.value,
      datasets: [
        {
          data: dataValues.value,
          backgroundColor: [
            "#77CEFF",
            "#0079AF",
            "#123E6B",
            "#97B0C4",
            "#A5C8ED",
          ],
        },
      ],
    }));

    const options = ref({
      responsive: true,
      plugins: {
        legend: {
          position: "top",
        },
        title: {
          display: true,
          text: "Vue-chart-3",
        },
      },
    });

    function shuffleData() {
      dataValues.value = shuffle(dataValues.value);
    }
    function addNum(){
      dataValues.value[0] += 5;
    }

    function addData(){
      console.log(this.chartData[0].value);
      // dataValues.value.push(this.chartData[0].value);
    }



    function handleChartRender(chart) {
      console.log(chart);
    }

    return { shuffleData, addNum, addData, testData, options, handleChartRender };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.wrapper{
  margin: 0 auto;
  width: 400px;
  text-align: center;
}
.doughnut-chart{

}
</style>
