<template>
  <form @submit.prevent>
    <my-input v-model="chartData.label">label</my-input>
    <my-input v-model="chartData.value">value</my-input>
    <button @click="createData">create</button>
  </form>

</template>

<script>
import MyInput from "./MyInput";

export default {
  name: "my-dialog",
  components: {
    MyInput
  },
  data(){
    return{
      chartData: {
        value: '',
        label: '',
      }
    }
  },
  methods: {
    createData(){
      this.$emit('create', this.chartData)
      this.chartData = {
        value: '',
        label: '',
      }
    }
  }
}
</script>

<style scoped>

</style>