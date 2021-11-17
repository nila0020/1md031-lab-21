<template>
  <div>
    <dt>
      <header><h3> {{burger.name}}</h3></header>
      <img v-bind:src="burger.img" height="190" width="240" alt="Good Burger"
           title="Good Burger" />
    </dt>
    <dd>
      <ul>
        <li>kCal: {{burger.kCal}} </li>
        <li v-if="burger.gluten"><span class="gluten" v-if="burger.gluten">Contains gluten</span></li>
        <li v-if="burger.lactose"><span class="lactose" >Contains lactose</span></li>
      </ul>
    </dd>
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
