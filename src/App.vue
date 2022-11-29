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
        <p v-if="sumOfDigits > 0">Прибыль</p>
        <p v-else-if="sumOfDigits < 0">Остаток</p>
        {{ sumOfDigits }} ₽
      </h1>
    </section>

    <section class="category_btns">
      <template v-if="obj.labelsValues">
        <div v-for="item in getColor"
             :key="item"
             class="category_btns-table"
        >
            <button :id="item" :style="{'border': `5px solid ${item['color']}`}">
              {{
                (item['label'] + '').length > 9 ? (item['label'] + '').slice(0,9) + '...' : (item['label'] + '')
              }}
            </button>
        </div>
      </template>
      <button class="show-form_btn" @click="toggleForm()">+</button>
    </section>
  </div>
</template>

<script>
import {computed, ref} from "vue";
import { concat } from "lodash";
import { DoughnutChart } from "vue-chart-3";
import ChartFormInput from "./components/UI/ChartFormInput";

export default {
  name: "App",
  components: { DoughnutChart, ChartFormInput},
  setup() {
    let numberValue = ref(null);
    let labelValue = ref('');
    let Transactions = ref([]);
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
      })
    );

    const options = ref({
      responsive: true,
      borderWidth: 0,
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
      let color = `#${(Math.random() * 0x1000000 | 0x1000000).toString(16).slice(1)}`

      Transactions.value.push(new Transaction(+(`${value}${numberValue.value}`), labelValue.value, color))

      obj.value.dataValues = concat(obj.value.dataValues, +(`${value}${numberValue.value}`));
      obj.value.labelsValues.push(labelValue.value);
      obj.value.colorsValues.push(color);

      numberValue.value = null;
      labelValue.value = null;
      showForm.value = false;
    }

    // function getColor(){
    //   console.log(Object.values(Transactions)[4]);
    //   return Object.values(Transactions)[4]
    //   // console.log(Object.values(Transactions)[4][1]?.color);
    // }

    function toggleForm(){
      showForm.value = true;
    }

    return {
      createData,
      toggleForm,
      showForm,
      obj,
      Transactions,
      numberValue,
      labelValue,
      chartData,
      options,
     };
  },
  computed: {
    sumOfDigits(){
      return this.obj.dataValues.reduce((a,b) => a + b, 0).toFixed(2)
    },
    getColor(){
      return Object.values(this.Transactions)
    }
  }
};
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
   margin-top: -215px;
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
  display: flex;
  flex-flow: column wrap;
  width: 400px;
  height: 200px;
  background-color: rgba(255, 0, 0, 0.29);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 5px 5px 20px rgba(0,0,0,0.5);
  position: absolute;
  border-radius: 8px;
  padding: 25px;
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
  transform: rotate(-6.82deg);
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
  background: rgba(0,0,0,.5);
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
  background-color: rgba(255,255,255,0.1);
  border: 1px solid rgba(255,255,255,0.25);
  outline: none;
  font-weight: normal;
  font-size: 16px;
  margin: 0 auto;
  width: 160px;
  height: 30px;
  border-radius: 5px;
  z-index: 6;
  /*background-color: rgba(255,255,255,0.1);*/
}
</style>
