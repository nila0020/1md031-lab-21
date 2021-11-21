<template>
  <header id="header">
    <img src="img/burger-joint.jpg" alt="The Burger Joint"/>
    <h1>Welcome to <br> The Burger Joint</h1>
  </header>
  <main >
    <section id="burger">
      <header>
        <h2>Our burger selection</h2>
      </header>
      Select your burgers:
      <div class="wrapper">
      <Burger v-for="burger in burgers"
              v-bind:burger="burger"
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>
      </div>
    </section>
    <section id="customer" style="clear:left">
      <header>
        <h2>Customer information</h2>
      </header>
      <p id="infoHeader">This is where you provide necessary information</p>
      <div id="contact">
        <header><h3>Delivery information:</h3></header>
        <form>
          <p>
            <label for="Fullname">Full name</label><br>
            <span v-if="RequiredFormsFilled">** Please fill in required field:<br></span>
            <input type="text" id="Fullname" v-model="Fullname" required="required" placeholder="First and Last name"/>
          </p>
          <p>
            <label for="Email">Email</label><br>
            <span v-if="RequiredFormsFilled">** Please fill in required field:<br></span>
            <input type="email" id="Email" v-model="Email" required="required" placeholder="Email address">
          </p>
          <!--<p>
            <label for="Street">Street</label><br>
            <input type="text" id="Street" v-model="Street" required="required" placeholder="Street name">
          </p>
          <p>
            <label for="House">House Nr</label><br>
            <input type="text" id="House" v-model="House" required="required" placeholder="House number">
          </p>-->
          <p>
            <label for="paymentmethod">Payment method</label><br>
            <select id="paymentmethod" v-model="pay_meth">
              <option  selected value="">Credit Card</option>
              <option>Swish</option>
              <option>Paypal</option>
              <option>American Express</option>
            </select>
          </p>
          <div>Gender</div>
          <div>
            <label for="Male">Male</label>
            <input type="radio" id="Male" v-model="gender" value="Male">
          </div>
          <div>
            <label for="Female">Female</label>
            <input type="radio" id="Female" v-model="gender" value="Female">
          </div>
          <div>
            <label for="Non-binary">Non-binary</label>
            <input type="radio" id="Non-binary" v-model="gender" value="Non-binary">
          </div>
          <div>
            <label for="Undiclosed">Undisclosed</label>
            <input type="radio" id="Undiclosed" v-model="gender" value="Undiclosed" checked>
          </div>
        </form>
      </div>
      <header><h3>Choose a delivery location:</h3></header>
      <span v-if="RequiredFormsFilled">** Please fill in required field:<br></span>
      <div class="mapwrapper">
        <div id="map" v-on:click="setLocation" >
          <div id="dots">
            <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}">
              T
            </div>
          </div>
        </div>
      </div>
    </section>
    <section >
      <button v-on:click="submit">
        <img src="img/Place_order_fixed.png" height="70" width="60"/>
      </button>
    </section>
    <section id="receipt">
      <header><h3>Your orders:</h3></header>
      <div v-if="receipt">
        {{orderedBurgers}}<br>
        Name: {{Fullname}}<br>
        Email: {{Email}}<br>
        Gender: {{gender}}<br>
        Pay method: {{pay_meth}}<br>
      </div>
    </section>
  </main>
  <footer>
    <hr>
    &copy; 2018 Hypothetical Burger Inc.
  </footer>
</template>

<script>
import Burger from '../components/Burger.vue'
import Menu from "../assets/menu.json"
import io from 'socket.io-client'

const socket = io();

/*function MenuItem (name, img, kcal, glut, lact) {
  this.name = name;
  this.img = img;
  this.kcal = kcal
  this.gluten = glut;
  this.lactose = lact;
}*/

const burgers = Menu /*[new MenuItem("Good burger", "pic", 300, true, true),
                new MenuItem("Fire burger", "pic", 400, true, true),
                new MenuItem("Vegan burger", "pic", 250, true, false) ]*/

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgers,
      Fullname: null,
      Email: null,
      /*Street: '',
      House: '',*/
      gender: 'undisclosed',
      pay_meth: '',
      orderedBurgers: {},
      location: { x: 0,
        y: 0
      },
      RequiredFormsFilled: false,
      receipt: false
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    /*addOrder: function () {
      let offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    },*/
    submit: function () {
      if (this.isRequiredFormsFilled()) {
        socket.emit("addOrder", {
              orderId: this.getOrderNumber(),
              details: {
                x: this.location.x,
                y: this.location.y,
                Fullname: this.Fullname,
                Email: this.Email,
                pay_meth: this.pay_meth,
                gender: this.gender
              },
              orderItem: this.orderedBurgers
            }
        )
        this.RequiredFormsFilled = false
        this.receipt = true
      }
      else{
        this.RequiredFormsFilled = true
      }

    },

    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },

    setLocation: function(event) {
      let offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    },
    isRequiredFormsFilled: function() {
      if (this.Fullname == null || this.Email == null || this.location.x === 0 || this.location.y === 0) {
        return false
      }
      else {
        return true
      }
    }
  }
}
</script>

<style>
/*Body and Header*/
body {
  font-family: "Times New Roman", serif;
  font-size: 12pt;
}
#header {
  margin-left: 50px;
  margin-right: 50px;
  overflow: hidden;
  height: 370px;
}
#header img {
  opacity: 0.45;
  width: 100%;
  height: auto;
}
#header h1 {
  position: absolute;
  margin-top: -330px;
  padding-left: 310px;
  font-size: 80pt;
  text-align: center;
  color: gold;
  text-shadow: 4px 3px black;
}
#infoHeader {
  border: 2px solid black;
}

/*Food content*/
.gluten {
  font-weight: bold;
}
.lactose {
  font-weight: bold;
}
.vegan {
  font-weight: bold;
}

/*ID:s*/
#burger {
  color: white;
  background-color: black;
  border: 2px dashed white;
  padding-left: 10px;
}
#customer {
  border: 2px dashed black;
  padding-left: 10px;
  margin-top: 5px;
}
#receipt {
  border: 2px solid black;
  padding-left: 10px;
  padding-bottom: 10px;
  margin-top: 5px;
}
.mapwrapper{
  max-height: 30em;
  overflow: scroll;

}

/*Buttons and selectors*/
button {
  margin-top: 20px;
  background-color: black;
}
button:hover {
  background-color: gold;
  cursor: pointer;
}
select:hover {
  background-color: white;
  cursor: pointer;
}

/*Structure and format*/
section {
  margin-left: 50px;
  margin-right: 50px;

}
.wrapper {
  display: grid;
  grid-template-columns: 20% 20% 20%;
}
.wrapper h3 {
  text-align: center;
  font-size: 1.5em;
}
#map {
  width: 1920px;
  height: 1078px;
  background: url("/img/polacks.jpg");

}
#dots {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}
</style>
