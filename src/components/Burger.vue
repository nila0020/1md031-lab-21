<template>
  <div>
    {{ burger.name }} {{burger.img}} {{ burger.kcal }} {{burger.gluten}} {{burger.lactose}}
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
