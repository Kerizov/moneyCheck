<template>
<div class="header">
  <button class="menu-btn"><img src="menu.svg" alt="menu"></button>
  <div>
    <ul>
      <li>Все счета<img src="row" alt="row"></li>
    </ul>
    <p class="currentMoney">15000</p>
  </div>
  <a href="#">Username</a>
</div>

</template>



<script>



export default {
  name: 'Header',
  props: {
    msg: String
  },
}
</script>

<style lang="scss" scoped>
.header{
  display: flex;
  justify-content: space-between;
  text-align: center;
}
.menu-btn{
  background-color: transparent;
  border: none;
}

</style>
