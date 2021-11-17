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
        <div  class="box a">
          <dt>
            <header><h3>Good Burger</h3></header>
            <img src="img/Good_burger.jpg" height="190" width="240" alt="Good Burger"
                 title="Good Burger" />
          </dt>
          <dd>
            <ul>
              <li>Contains <span class="gluten">gluten</span></li>
              <li>Contains <span class="lactos">lactose</span></li>
            </ul>
          </dd>
        </div>
        <div class="box b">
          <dt>
            <header><h3>Fire Burger</h3></header>
            <img src="img/Fire_burger.jpg" height="190" width="240" alt="Fire Burger" title="Fire Burger"/>
          </dt>
          <dd>
            <ul>
              <li>Contains <span class="gluten">gluten</span></li>
              <li>Contains <span class="lactos">lactose</span></li>
            </ul>
          </dd>
        </div>
        <div class="box c">
          <dt>
            <header><h3>Vegan Burger</h3></header>
            <img src="img/Vegan_burger.jpg" height="190" width="240" alt="Vegan Burger" title="Vegan Burger"/>
          </dt>
          <dd>
            <ul>
              <li>Contains <span class="gluten">gluten</span></li>
              <li><span class="vegan">Vegan</span></li>
            </ul>
          </dd>
        </div>
      </div>
    </section>
    <section id="customer" style="clear:left">
      <header>
        <h2>Customer information</h2>
      </header>
      This is where you provide necessary information
      <div id="contact">
        <header><h3>Delivery information:</h3></header>
        <form>
          <p>
            <label for="Fullname">Full name</label><br>
            <input type="text" id="Fullname" name="Fullname" required="required" placeholder="First and Last name">
          </p>
          <p>
            <label for="Email">Email</label><br>
            <input type="email" id="Email" name="Email" required="required" placeholder="Email address">
          </p>
          <p>
            <label for="Street">Street</label><br>
            <input type="text" id="Street" name="Street" required="required" placeholder="Street name">
          </p>
          <p>
            <label for="House">House Nr</label><br>
            <input type="text" id="House" name="House" required="required" placeholder="House number">
          </p>
          <p>
            <label for="paymentmethod">Payment method</label><br>
            <select id="paymentmethod" name="pay_meth">
              <option selected="selected">Credit Card</option>
              <option>Swish</option>
              <option>Paypal</option>
              <option>American Express</option>
            </select>
          </p>
          <div>Gender</div>
          <div>
            <label for="male">Male</label>
            <input type="radio" id="male" name="gender" value="male">
          </div>
          <div>
            <label for="female">Female</label>
            <input type="radio" id="female" name="gender" value="female">
          </div>
          <div>
            <label for="non-binary">Non-binary</label>
            <input type="radio" id="non-binary" name="gender" value="non-binary">
          </div>
          <div>
            <label for="undiclosed">Undisclosed</label>
            <input type="radio" id="undiclosed" name="gender" value="undiclosed" checked>
          </div>
        </form>
      </div>
    </section>
    <button type="submit">
      <img src="img/Place_order_fixed.png" height="70" width="60"/>
    </button>
  </main>
  <footer>
    <hr>
    &copy; 2018 Hypothetical Burger Inc.
  </footer>
</template>

<script>
//import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

const socket = io();

function MenuItem (name, img, kcal, glut, lact) {
  this.name = name;
  this.img = img;
  this.kcal = kcal
  this.gluten = glut;
  this.lactose = lact;
}

const burgers = [new MenuItem("Good burger", "pic", 300, true, true),
                new MenuItem("Fire burger", "pic", 400, true, true),
                new MenuItem("Vegan burger", "pic", 250, true, false) ]

export default {
  name: 'Home',
  components: {
    //Burger
  },
  data: function () {
    return {
      burgers: burgers
                /* [ {name: "small burger", kCal: 250},
                 {name: "standard burger", kCal: 450},
                 {name: "large burger", kCal: 850} ]*/
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      let offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
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

/*Food content*/
.gluten {
  font-weight: bold;
}
.lactos {
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

/*Buttons and selectors*/
button {
  margin-left: 50px;
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
</style>
