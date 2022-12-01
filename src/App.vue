<template>
  <div :class="{'fade': showForm === true}">
    <form class="chart_form-data" v-if="showForm" @submit.prevent>
      <ChartFormInput v-model="labelValue" placeholder="Наименование"></ChartFormInput>
      <ChartFormInput v-model.number="numberValue" placeholder="Кол-во"></ChartFormInput>
      <div style="display: flex; justify-content: space-between">
        <button @click="createData('+')">Прибыль</button>
        <button @click="createData('-')">Трата</button>
      </div>
    </form>

    <section class="chart">
      <DoughnutChart
          class="doughnut-chart"
          :data="chartData"
          :options="options"
      />
      <h1 class="chart_center-text">
        <p v-if="sumOfDigits() > 0">Доход</p>
        <p v-else-if="sumOfDigits() < 0">Остаток</p>
        {{ sumOfDigits() }} $
      </h1>
    </section>

    <section class="category_btns">
      <template v-if="obj.labelsValues">
        <div v-for="item in getColor()"
             :key="item"
             class="category_btns-table"
        >
            <button
                :id="item"
                :style="{'border': `5px solid ${item['color']}`}"
            >
              {{(item['label'] + '').length > 9 ? (item['label'] + '').slice(0,9) + '...' : (item['label'] + '') }}
            </button>
        </div>
        <button v-if="obj.labelsValues.length < 15" class="show-form_btn" @click="toggleForm()">+</button>
      </template>

    </section>
  </div>
</template>

<script setup>
import {computed, ref} from "vue";
import {concat} from "lodash";
import {DoughnutChart} from "vue-chart-3";
import ChartFormInput from "./components/UI/ChartFormInput";

    let numberValue = ref(null);
    let labelValue = ref(null);
    let transactions = ref([]);
    let obj = ref({
      labelsValues: [],
      dataValues: [],
      colorsValues: [],
    });
    let showForm = ref(false);

    function Transaction(data,label,color) {
      this.id = Date.now();
      this.data = [data]
      this.label = [label]
      this.color = color
    }

    const chartData = computed(() => ({
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
      borderWidth: 0.1,
      borderColor: '#646464',
      cutout: '80%',
      plugins: {
        legend: {
          display: false
        },
        title: {
          display: false,
          text: "Vue-chart-3",
        },
      },
    });

    function createData(value){
      let color = `${"hsl(" + 360 * Math.random() + ',' + 100 + '%,' + 50 + '%)'}`

      transactions.value.push(new Transaction(+(`${value}${numberValue.value}`), labelValue.value, color))

      obj.value.dataValues = concat(obj.value.dataValues, +(`${value}${numberValue.value}`));
      obj.value.labelsValues.push(labelValue.value);
      obj.value.colorsValues.push(color);

      numberValue.value = null;
      labelValue.value = null;
      showForm.value = false;
    }

    function toggleForm(){
      showForm.value = true;
    }

    function sumOfDigits(){
      return computed(() => obj.value.dataValues.reduce((a, b) => a + b, 0).toFixed(2)).value
    }

    function getColor(){
      return computed(() => Object.values(transactions.value)).value
    }
</script>

<style>
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  text-align: center;
  color: #fff;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100vh;
  width: 100vw;
  padding-top: 100px;
  color: #2c3e50;
  background: linear-gradient(199.76deg, #030083 29.39%, #000A2E 49.65%, #0A001F 72.65%);
}
section.chart{
  position: relative;
  margin: 0 auto;
  width: 400px;
  text-align: center;
}
.chart_center-text{
   margin-top: -230px;
}
.chart_center-text p{
  font-size: 24px;
  font-weight: 400;
}
.category_btns{
  margin: 150px auto;
  width: 700px;
  padding-top: 100px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.category_btns button{
  background-color: transparent;
  width: 100px;
  height: 100px;
  border-radius: 50%;
  border: none;
  font-size: 16px;
  margin: 10px;
}
.category_btns-table{
  display: flex;
}
button.show-form_btn{
  background-color: #646464;
  color: #373737;
  font-size: 48px;
}
.chart_form-data{
  transition: transform .8s ease-in-out;
  display: flex;
  flex-flow: column wrap;
  width: 400px;
  height: 200px;
  background-color: rgba(255, 0, 0, 0.29);
  box-shadow: 5px 5px 20px rgba(0,0,0,0.5);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  position: absolute;
  border-radius: 8px;
  padding: 40px 25px 0px 25px;
  z-index: 10;
}
.chart_form-data:after{
  content: '';
  width: 400px;
  height: 200px;
  position: absolute;
  border-radius: 8px;
  top: 0;
  left: 0;
  transform: rotate(-3.82deg);
  background-color: rgba(57, 88, 170, 0.29);
  z-index: 5;
}
.fade:after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,.9);
  z-index: 5;
}
.chart_form-data input{
  background-color: rgba(255,255,255,0.1);
  border: 1px solid rgba(255,255,255,0.25);
  outline: none;
  font-size: 16px;
  font-weight: normal;
  padding: 5px;
  text-align: left;
  margin-bottom: 10px;
  height: 30px;
  width: 100%;
  z-index: 6;
}
.chart_form-data button{
  border: 1px solid rgba(255,255,255,0.25);
  outline: none;
  font-weight: normal;
  font-size: 16px;
  margin: 0 auto;
  width: 160px;
  height: 30px;
  border-radius: 5px;
  z-index: 6;
}
.chart_form-data button:first-child{
  background-color: rgba(30, 255, 0, 0.5);
}
.chart_form-data button:last-child{
  background-color: rgb(255, 0, 0, 0.5);
}
</style>
