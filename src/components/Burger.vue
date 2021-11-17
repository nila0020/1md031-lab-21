<template>
  <div id="burger">
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
    <button v-on:click="Add_burger">Add burger</button>
    <button v-on:click="Remove_burger">Remove burger</button>
    <br>
    <p id="Amount">Amount: {{amountOrdered}}</p>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    Add_burger: function () {
      this.amountOrdered ++
    },
    Remove_burger: function () {
      if (this.amountOrdered > 0) {
        this.amountOrdered --
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#burger {
  border: 5px outset gray;
}
#Amount {
  text-align: center;
}
button {
  background-color: gray;
}
</style>
