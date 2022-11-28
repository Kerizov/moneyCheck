<template>

  <form class="form-data" id="addForm" @submit.prevent>
    <my-input v-model="labelValue">label</my-input>
    <my-input v-model.number="numberValue">value</my-input>
    <button @click="createData">create</button>
  </form>
  <div class="wrapper">
    <!--    <my-dialog @create="createData"></my-dialog>-->

    <DoughnutChart
        class="doughnut-chart"
        :data="testData"
        :options="options"
        @chart:render="handleChartRender"
    />
    <h1 class="sum-of-money">{{ sumOfDigits }}</h1>
  </div>
  <div class="chart-btn" >
    <!--      <button :id="item" @click="toggleData(0)">{{ item }}</button>-->
    <!--      <button id="nîmes" @click="toggleData(1)">second</button>-->
    <!--      <button id="toulon" @click="toggleData(2)">third</button>-->
    <!--      <button id="perpignan" @click="toggleData(3)">four</button>-->
    <button @click="showForm">+</button>
  </div>


</template>

<script>
import {computed, ref, onMounted} from "vue";
import { concat } from "lodash";
import { DoughnutChart } from "vue-chart-3";

// import MyDialog from "./components/UI/MyDialog";
import MyInput from "./components/UI/MyInput";

export default {
  name: "App",
  components: { DoughnutChart, MyInput},
  setup() {
    let numberValue = ref(null);
    let labelValue = ref('');
    let obj = ref({
      labelsValues: [" "],
      dataValues: [1],
      colorsValues: ["#77CEFF"],
    });

    const testData = computed(() => ({
      labels: obj.value.labelsValues,
      datasets: [
        {
          data: obj.value.dataValues,
          backgroundColor: obj.value.colorsValues,
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
      // document.getElementById('paris').style.backgroundColor = testData.value.datasets[0].backgroundColor[0];
      // document.getElementById('nîmes').style.backgroundColor = testData.value.datasets[0].backgroundColor[1];
      // document.getElementById('toulon').style.backgroundColor = testData.value.datasets[0].backgroundColor[2];
      // document.getElementById('perpignan').style.backgroundColor = testData.value.datasets[0].backgroundColor[3];
      // document.getElementById('paris').innerText = testData.value.labels[0];
      // document.getElementById('nîmes').innerText = testData.value.labels[1];
      // document.getElementById('toulon').innerText = testData.value.labels[2];
      // document.getElementById('perpignan').innerText = testData.value.labels[3];
    })

    function createData(){
      obj.value.dataValues = concat(obj.value.dataValues, numberValue.value);
      obj.value.labelsValues.push(labelValue.value);
      numberValue.value = null;
      labelValue.value = null;
      console.log(obj.value);
      document.getElementById('addForm').style.display = 'none';
      let color = Math.floor(Math.random()*0xFFFFFF<<0).toString(16);
      testData.value.datasets[0].backgroundColor.push(`#${color}`);
      // testData.value.datasets[0].backgroundColor[0] = 'dd'
    }
    function toggleData(value){
      console.log(value);
    }
    function handleChartRender(chart) {
      console.log(chart);
    }

    function showForm(){
      document.getElementById('addForm').style.display = 'flex';
      // console.log(Math.floor(Math.random()*0xFFFFFF<<0).toString(16));
    }



    return {
      createData,
      toggleData,
      showForm,
      obj,
      numberValue,
      labelValue,
      testData,
      options,
      handleChartRender };
  },
  computed: {
    sumOfDigits(){
      return this.obj.dataValues.reduce((a,b) => a + b, 0)
    },
  }

};
</script>

<style>
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.wrapper{
  position: relative;
  margin: 0 auto;
  width: 400px;
  text-align: center;
}
.chart-btn{
  margin: 0 auto;
  width: 630px;
  padding-top: 100px;
  display: flex;
  justify-content: center;
}
.chart-btn button{
  background-color:#000;
  color: #ffffff;
  width: 100px;
  height: 100px;
  border-radius: 50%;
  border: none;
  font-size: 18px;
}
.sum-of-money{
  margin: 0;
  padding: 0;
  position: absolute;
  top: 45%;
  right: 45%;
  bottom: 45%;
  left: 45%;

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
  box-shadow: 5px 5px 20px rgba(0,0,0,0.5);
  position: absolute;
  border-radius: 5px;
  padding: 25px;
  z-index: 10;
}
.form-data input{
  margin-bottom: 10px;
  height: 30px;
  border: 1px solid #ccc;
  width: 100%;


}
</style>
