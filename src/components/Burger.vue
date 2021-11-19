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


    <br>
    <p id="Amount">Amount: <button v-on:click="removeBurger">-</button> {{amountOrdered}} <button v-on:click="addBurger">+</button></p>
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
    addBurger: function () {
      this.amountOrdered ++
      this.$emit('orderedBurger', {name: this.burger.name,
                                    amount: this.amountOrdered}
      );
    },
    removeBurger: function () {
      if (this.amountOrdered > 0) {
        this.amountOrdered --
        this.$emit('orderedBurger', {name: this.burger.name,
          amount: this.amountOrdered}
        )
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
  font-size: 1.5em;
}
button {
  background-color: gray;
}
</style>
