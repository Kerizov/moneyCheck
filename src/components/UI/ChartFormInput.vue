<template>
  <input type="text" :value="modelValue" :placeholder="$props.placeholder" @input="updateInput">
</template>

<script>
export default {
  name: "ChartFormInput",
  props: {
    modelValue: [String, Number],
    placeholder: String
  },
  methods: {
    updateInput(e){
      this.$emit('update:modelValue', e.target.value)
    }
  }
}
</script>

<style scoped>
::placeholder {
  color: rgba(255, 255, 255, 0.25);
  /*opacity: 1;*/
}
</style>