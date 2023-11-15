<template>
  <header>
    <img
      src="https://tasteandflavors.com/wp-content/uploads/2018/11/taste-and-flavors-out-and-about-10-best-burgers-wide.jpg"
      alt="FancyBurgir" id="headpic">
    <h1 style="position: absolute;">
      Welcome to Burgers Online
    </h1>
  </header>

  <main>
    <section class="menu">
      <h2>
        Select Your Burger
      </h2>
      ###insert options and so forth
      <h2>
        Menu
      </h2>
      <div class="wrapper">
        <Burger v-for="burger in menu" :key="burger.name" :burger="burger" @orderedBurgers="addToOrder($event)" />
      </div>

    </section>

    <section id="customerInformation">
      <h3>
        Customer information
      </h3>
      <div>
        <p>Full name</p>
        <input v-model="fullName" placeholder="First- and Last name" />

        <p>E-mail</p>
        <input v-model="email" placeholder="Email Address" />

        <p>Payment</p>
        <select v-model="paymentMethod">
          <option>Card</option>
          <option selected>Swish</option>
          <option>Klarna</option>
          <option>Cash</option>
        </select>

        <p>Gender</p>
        <input type="radio" id="male" name="gender" value="Male" v-model="gender" />
        <label for="male">Male</label>
        <br>
        <input type="radio" id="female" name="gender" value="Female" v-model="gender" />
        <label for="female">Female</label>
        <br>
        <input type="radio" id="other" name="gender" value="Do not want to disclose" v-model="gender" />
        <label for="other">Do not want to disclose</label>
      </div>

      <div class="delivary-location">
        <h3 id="setLocation">
          Set your location:
        </h3>
        <div id="map" v-on:click="setLocation" style="position:relative">
          <div id="target" v-bind:style="{ position: 'absolute', left: location.x + 'px', top: location.y + 'px' }">
            T
          </div>
        </div>
      </div>

    </section>

    <button type="submit" v-on:click="addOrder()"><img
        src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/221a2a78-3338-44c0-b21a-3f2a66d031ae/d7kn14u-6bfbf8dd-15a1-4cc5-8b4b-aa5fca06fa97.gif?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcLzIyMWEyYTc4LTMzMzgtNDRjMC1iMjFhLTNmMmE2NmQwMzFhZVwvZDdrbjE0dS02YmZiZjhkZC0xNWExLTRjYzUtOGI0Yi1hYTVmY2EwNmZhOTcuZ2lmIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.8XfI0kd52tXCECHEl3OaUozYb5XpAEhc2HlbMnSkt8g"
        style="width: 25px;">
      Send Order
    </button>

  </main>
  <footer>
    <hr>
    <code> Copyright procteced (footer) </code>
  </footer>
</template>

<script>
import menu from '../assets/menu.json'
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io();


//function MenuItem(name, imageUrl, kCal, containsGluten, containsLactose) {
//this.name = name;
//this.imageUrl = imageUrl;
//this.kCal = kCal;
//this.containsGluten = containsGluten;
//this.containsLactose = containsLactose;
//}

export default {
  name: 'HomeView',
  components: {
    // eslint-disable-next-line vue/no-unused-components
    Burger,
  },
  data: function () {
    return {
        fullName: '',
        email: '',
        gender: '',
        paymentMethod: '',
      orderedBurgers: {},
      picked: 'Do not want to disclose',
      selected: "Swish",
      menu,
      location: {
        x: 0,
        y: 0
      },
      orderId: 0,
    }
  },

  methods: {
    addToOrder: function (burger) {
      this.orderedBurgers[burger.name] = burger.amount;
    },
    addOrder: function () {
      console.log('Name:', this.fullName);
      console.log('Email:', this.email);
      console.log('Payment Method:', this.paymentMethod);
      console.log('Gender:', this.gender);
      console.log('Ordered Burgers:', this.orderedBurgers);
      console.log('Location:', this.location)
      console.log('Order ID:', this.getOrderNumber());
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),

        customer: {
          name: this.fullName,
          email: this.email,
          paymentMethod: this.paymentMethod,
          gender: this.gender,
        },

        details: {
          location: {
            x: this.location.x,
            y: this.location.y
          },

          orderItems: this.orderedBurgers
        }
      });
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location = {
        x: event.clientX - 60 - offset.x,
        y: event.clientY - 60 - offset.y,
      };
    },
  }
}
</script>

<style>
body {
  font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}

.allergies {
  font-weight: bold;
}

button {
  background-color: lightgray;
  margin: 10px 15px;
  text-align: center;
  border-style: hidden;
  border-radius: 5%;
  padding: 5px 10px;
}

button:hover {
  background-color: rgb(12, 201, 12);
  cursor: pointer;
}

header,
section {
  margin: 5px 10px;
}

section {
  border-style: dashed;
  border-radius: 20px;
}

section>p,
form {
  margin-left: 15px;
}

div {
  padding: 5px;
  margin: 50px;
}

#headpic {
  opacity: 0.8;
  width: 100%;
  height: auto;
}

header {
  height: 600px;
  overflow: hidden;
}

h1 {
  width: 88%;
  text-align: center;
  margin-top: -37%;
}

.menu {
  background-color: black;
  color: white;
}

.box {
  margin: 10px;
}

.wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

#contains {
  text-align: center;
}

.image-container {
  margin: 0px;
  width: 200px;
  height: 200px;
  overflow: hidden;
}

.image-container>img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}


header>.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

#map {
  width: 1920px;
  height: 1078px;
  background: url("C:\Users\Pontus\Downloads\lab1-granssnitt\public\img\polacks.jpg");
  background-size: cover;
  /*position: absolute;*/
}

.map-container {
  width: 100%;
  height: 100%;
  overflow: scroll;
  position: relative;

}

#setLocation {
  text-align: center;
}
</style>