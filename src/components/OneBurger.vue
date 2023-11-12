<template>
  <div class="box">
    <h3>{{ menu.name }}</h3>
    <img :src="burger.img" alt="Burger Image" title="burger.name" style="width: 200px;">
    <br> 
    <h4>
        Contains:
    </h4>
    <ul>
        <li v-if="burger.containsGluten"> <span class="allergies">Gluten </span> </li>
        <li v-if="burger.containsLactose"> <span class="allergies">Lactose </span> </li>
        <li v-if="!burger.containsGluten && !burger.containsLactose"> No allergens </li>
    </ul>  
    <div>
      {{ burger.name }} {{ burger.kCal }} kCal
    </div>

    <p>Amount: {{ burger.amountOrdered }}</p>
            <button type="add" v-on:click="addBurger" style="box-sizing: 25px;">
              +
            </button>
            <button type="remove" v-on:click="removeBurger" style="box-sizing: 25px;">
              -
            </button>
  </div>
</template>

  <script>
  import menu from '../assets/menu.json'

  export default {
    name: 'OneBurger',
    orderedBurgers: {},
    props: {
      burger: Object
    },
    data: function(){
      return{
        amountOrdered:0,
        menu
      }
    },
    methods: {
    addBurger: function() {
      this.amountOrdered += 1;
      this.$emit('orderedBurgers', {
        name: this.burger.name,
        amount: this.amountOrdered,
      });
    },
    removeBurger: function(){
      if (this.burger.amountOrdered > 0) {
        this.amountOrdered --;
        this.$emit('orderedBurgers', {
          name: this.burger.name,
          amount: this.amountOrdered})
      }
    },
  }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  body {
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}

.allergies{
    font-weight: bold;
}
.box {
    margin: 50px;
}
  </style>
  