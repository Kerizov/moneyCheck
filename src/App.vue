<template>
  <div :class="{'fade' : showForm === true}">
    <form class="chart_form-data" v-if="showForm" @submit.prevent>
      <ChartFormInput v-model="labelValue" placeholder="Наименование"></ChartFormInput>
      <ChartFormInput v-model.number="numberValue" type="number" placeholder="Кол-во"></ChartFormInput>
      <div class="chart_form-data-color">
        <label>Выберите цвет:</label>
        <ChartFormInput v-model="colorValue" type="color"></ChartFormInput>
      </div>

      <div>
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
        <p v-if="getSumOfTransactions() > 0">Доход</p>
        <p v-else-if="getSumOfTransactions() < 0">Остаток</p>
        {{ getSumOfTransactions() }} $
      </h1>
    </section>

    <section class="category_btns">
      <template v-if="transactions">
        <div v-for="item in getCategory()"
             :key="item"
             class="category_btns-table"
        >
            <button
                :id="item"
                :style="{'border': `5px solid ${item['color']}`}"
            >
              {{(item['data'] + '').length > 8 ? (item['data'] + '').slice(0,8) + '...' : (item['data'] + '') }}
              <br>
              {{(item['label'] + '').length > 8 ? (item['label'] + '').slice(0,8) + '...' : (item['label'] + '') }}
            </button>
        </div>
        <button v-if="transactions.length < 15" class="show-form_btn" @click="toggleForm()">+</button>
      </template>

    </section>
  </div>
</template>

<script setup>
import {computed, ref} from "vue";
import {DoughnutChart} from "vue-chart-3";
import ChartFormInput from "./components/UI/ChartFormInput";

    let numberValue = ref(null),
        labelValue = ref(null),
        colorValue = ref(null);

    let transactions = ref([]);
    let showForm = ref(false);

    function Transaction(data, label, color) {
      this.id = Date.now();
      this.data = [data]
      this.label = [label]
      this.color = color
    }

    const chartData = computed(() => ({
        labels: getLabel().value,
        datasets: [
          {
            data: getData().value,
            backgroundColor: getColor().value,
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

    function createData(operand){
      transactions.value.push(new Transaction(+(`${operand}${numberValue.value}`), labelValue.value, colorValue.value))

      numberValue.value = null;
      labelValue.value = null;
      showForm.value = false;
    }

    function toggleForm(){
      showForm.value = true;
    }

    function getSumOfTransactions(){
      return computed(() => [...getData().value].reduce((a, b) => a + b, 0).toFixed(2)).value
    }

    function getData(){
      return computed( () => Object.values(transactions.value).map(i => i['data'][0]))
    }

    function getLabel(){
      return computed(() => Object.values(transactions.value).map(i => i['label'][0]))
    }

    function getColor(){
      return computed(() => Object.values(transactions.value).map(i => i['color']))
    }

    function getCategory(){
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
body{
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: linear-gradient(199.76deg, #030083 29.39%, #000A2E 49.65%, #0A001F 72.65%) fixed;
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
  margin: 150px auto 0 auto;
  max-width: 700px;
  padding: 100px 20px;
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
  height: 240px;
  background-color: rgba(255, 0, 0, 0.29);
  box-shadow: 5px 5px 20px rgba(0,0,0,0.5);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  position: fixed;
  border-radius: 8px;
  padding: 40px 25px 0 25px;
  z-index: 10;
}
.chart_form-data > div{
  display: flex;
}
.chart_form-data-color{
  display: flex;
  justify-content: left;
  align-items: center;
  margin: 0 7px;
}
.chart_form-data:after{
  content: '';
  width: 400px;
  height: 240px;
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
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: -100px;
  background: rgba(0,0,0,0.9) fixed;
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
