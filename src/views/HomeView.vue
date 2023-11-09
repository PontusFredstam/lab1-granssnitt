<template>
 <header>
        <img src="https://tasteandflavors.com/wp-content/uploads/2018/11/taste-and-flavors-out-and-about-10-best-burgers-wide.jpg" alt="FancyBurgir" id="headpic">
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
          <div class="box" v-for="burger in burgers" :key="burger.name">
            <h3>{{ burger.name }}</h3>
            <div class="image-container">
              <img :src="burger.imageUrl" alt="Burger Image" class="image" style="width: 200px;">
            </div>
            <br>
            <h4 id="contains">Contains:</h4>
            <ul>
              <li v-if="burger.containsGluten">
                <span v-if="burger.containsGluten" class="allergies">Gluten</span></li><br>
              <li v-if="burger.containsLactose">
                <span v-if="burger.containsLactose" class="allergies">Lactose</span>
              </li>
              <li v-if="!burger.containsGluten & !burger.containsLactose">No allergens</li>
            </ul>
          </div>
        </div>
    </section>

    <section id="customerInformation">
    <h3> 
        Customer information
    </h3>
    <p>
        <label for="name"> Full name</label><br>
        <input type="text" id="name" name="fn" required="required" placeholder="First- and Last name">
    </p>
    <p>
        <label for="email"> E-mail</label><br>
        <input type="email" id="email" name="em" required="required" placeholder="Email Adress">
    </p>
    <p>
        <label for="street"> Street</label><br>
        <input type="text" id="street" name="Street" required="required" placeholder="Street Name">
    </p>
    <p>
        <label for="houseNumber"> House</label><br>
        <input type="number" id="houseNumber" name="houseNumber" required="required" placeholder="House number">
    </p>
    <p>
        <label for="payment"> Payment</label><br>
        <select id="payment" name="payment">
            <option selected> Swish</option>
            <option> Card payment</option>
            <option> Cash</option>
            <option> Klarna</option>
        </select>
    </p>
    <form>
        <h4>
            Gender:
        </h4>
        <label>
            <input type="radio" name="gender" value="male">
            Male
        </label><br>
        <label>
            <input type="radio" name="gender" value="female">
            Female
        </label><br>
        <label>
            <input type="radio" name="gender" value="Do not wish to provide" checked>
            Do not wish to provide
        </label>
    </form>
    </section>

    <button type="submit"><img src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/221a2a78-3338-44c0-b21a-3f2a66d031ae/d7kn14u-6bfbf8dd-15a1-4cc5-8b4b-aa5fca06fa97.gif?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcLzIyMWEyYTc4LTMzMzgtNDRjMC1iMjFhLTNmMmE2NmQwMzFhZVwvZDdrbjE0dS02YmZiZjhkZC0xNWExLTRjYzUtOGI0Yi1hYTVmY2EwNmZhOTcuZ2lmIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.8XfI0kd52tXCECHEl3OaUozYb5XpAEhc2HlbMnSkt8g" style="width: 25px;">
        Send Order
    </button>
    
    </main> 
    <footer>
        <hr>
        <code> Copyright procteced (footer) </code>
    </footer>
  <div>
    <div id="map" v-on:click="addOrder">
      click here
    </div>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io();


function MenuItem(name, imageUrl, kCal, containsGluten, containsLactose) {
    this.name = name;
    this.imageUrl = imageUrl;
    this.kCal = kCal;
    this.containsGluten = containsGluten;
    this.containsLactose = containsLactose;
}

  const burgerArray = [];
  
  const BBQBurger = new MenuItem("BBQ Burger", "https://assets.tmecosys.com/image/upload/t_web767x639/img/recipe/ras/Assets/102cf51c-9220-4278-8b63-2b9611ad275e/Derivates/3831dbe2-352e-4409-a2e2-fc87d11cab0a.jpg", 650, true, true );
  const meatLover = new MenuItem("Meat Lover", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQNfbgZdQMbfIP2v7MWpYKDn2kMIUsVzlw8rg&usqp=CAU", 2500, true, true );
  const veganHeaven = new MenuItem("Vegan Heaven", "https://www.inspiredtaste.net/wp-content/uploads/2018/05/Homemade-Mushroom-Veggie-Burger-Recipe-1200.jpg", 0, false, false);

  burgerArray.push(BBQBurger, meatLover, veganHeaven);
  console.log(burgerArray);

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers:burgerArray
    }
  },

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
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
body {
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}

.allergies{
    font-weight: bold;
}

button {
    background-color: lightgray;
    margin: 10px 15px;
    text-align: center;
    border-style:hidden;
    border-radius:5%;
    padding:5px 10px;
}

button:hover {
    background-color: rgb(12, 201, 12);
    cursor:pointer;
 }

header, section {
    margin: 5px 10px;
}

section {
    border-style:dashed;
    border-radius: 20px;
    }

section > p, form {
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
    overflow:hidden;
}

h1 {
    width: 88%;
    text-align: center;
    margin-top:-37%;
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
  margin:0px;
    width: 200px;
    height: 200px;
    overflow: hidden;
}

.image-container > img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}


header > .image {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

#map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>