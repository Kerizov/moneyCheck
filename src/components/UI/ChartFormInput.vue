<template>
  <input :type="$props.type" :value="modelValue" :placeholder="$props.placeholder" @input="updateInput">
</template>

<script>
export default {
  name: "ChartFormInput",
  props: {
    modelValue: [String, Number],
    placeholder: String,
    type: String,
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
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type="color"] {
  -webkit-appearance: none;
  border: none;
  background: transparent;
  width: 40px;
  height: 40px;
  overflow: hidden;
  outline: none;
  cursor: inherit;
  margin: 0 10px;
}
/* -webkit */
input[type="color"]::-webkit-color-swatch-wrapper {
  padding: 0;
}
input[type="color"]::-webkit-color-swatch {
  border: none;
}

</style>