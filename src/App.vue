<template>

    <form class="form-data" id="addForm" @submit.prevent>
      <my-input v-model="labelsValues">label</my-input>
      <my-input v-model="testValue">value</my-input>

      <button @click="createData">create</button>
    </form>
  <div class="wrapper">
    <button type="button" @click="shuffleData">Shuffle</button>
<!--    <my-dialog @create="createData"></my-dialog>-->

    <DoughnutChart
        class="doughnut-chart"
        :data="testData"
        :options="options"
        @chart:render="handleChartRender"
    />
  </div>
    <div class="chart-btn">
      <button id="paris" @click="toggleData(0)">first</button>
      <button id="nîmes" @click="toggleData(1)">second</button>
      <button id="toulon" @click="toggleData(2)">third</button>
      <button id="perpignan" @click="toggleData(3)">four</button>
      <button @click="showForm">+</button>
    </div>


</template>

<script>
import { computed, ref, onMounted } from "vue";
import { shuffle, concat } from "lodash";
import { DoughnutChart } from "vue-chart-3";

// import MyDialog from "./components/UI/MyDialog";
import MyInput from "./components/UI/MyInput";

export default {
  name: "App",
  components: { DoughnutChart, MyInput},
  setup() {

    let labelsValues = ref(["Paris", "Nîmes", "Toulon", "Perpignan"]);
    let testValue = ref([]);
    let dataValues = ref([30, 40, 60, 70]);

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

          ],
        },
      ],
    }));

    const options = ref({
      responsive: true,
      borderWidth: 0,
      cutout: '80%',
      plugins: {
        legend: {
          // position: "top",
          display: false
        },
        title: {
          display: false,
          text: "Vue-chart-3",
        },
      },
    });

    onMounted(() => {
      document.getElementById('paris').style.backgroundColor = testData.value.datasets[0].backgroundColor[0];
      document.getElementById('nîmes').style.backgroundColor = testData.value.datasets[0].backgroundColor[1];
      document.getElementById('toulon').style.backgroundColor = testData.value.datasets[0].backgroundColor[2];
      document.getElementById('perpignan').style.backgroundColor = testData.value.datasets[0].backgroundColor[3];
      document.getElementById('paris').innerText = testData.value.labels[0];
      document.getElementById('nîmes').innerText = testData.value.labels[1];
      document.getElementById('toulon').innerText = testData.value.labels[2];
      document.getElementById('perpignan').innerText = testData.value.labels[3];
    })


    function shuffleData() {
      dataValues.value = shuffle(dataValues.value);
    }

    function createData(){
      dataValues.value = concat(dataValues.value, testValue.value);
      testValue.value = '';
      document.getElementById('addForm').style.display = 'none';
    }
    function toggleData(value){
      console.log(value);
    }
    function handleChartRender(chart) {
      console.log(chart);
    }

    function showForm(){
      document.getElementById('addForm').style.display = 'flex';
    }

    return { shuffleData, createData, dataValues, toggleData, showForm, labelsValues, testValue, testData, options, handleChartRender };
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
.chart-btn{
  margin: 0 auto;
  width: 630px;
  padding-top: 100px;
  display: flex;
  justify-content: space-between;
}
.chart-btn button{
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background-color: red;
  border: none;
  font-size: 18px;
}
.form-data{
  display: none;
  flex-flow: column wrap;
  width: 400px;
  height: 300px;
  background-color: #fff;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 5px 5px rgba(0,0,0,0.5);
  position: absolute;
}
.form-data input{
  margin: 10px auto;
  height: 30px;
  width: 150px;

}
</style>
