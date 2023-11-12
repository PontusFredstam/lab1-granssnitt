<template>
  <div class="box">
    <h3>{{ burger.name }}</h3>
    <img v-bind:src="burger.imageUrl" alt="Burger Image" title="burger.name" style="width: 200px;">
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
      {{ burger.kCal }} kCal

      <p>Amount: {{ amountOrdered }}</p>
      <div class="button-container">
            <button type="add" v-on:click="addBurger(burger)" style="box-sizing: 25px;">
              +
            </button>
            <button type="remove" v-on:click="removeBurger(burger)" style="box-sizing: 25px;">
              -
            </button>
        </div>
    </div>
  </div>
</template>

  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    data: function() {
      return {
        amountOrdered:0,
      }
    },

    methods: {
  addBurger: function () {
    this.amountOrdered += 1;
    this.$emit('orderedBurgers', {
       name: this.burger.name,
       amount: this.amountOrdered });
  },

  removeBurger: function () {
    if (this.amountOrdered > 0) {
      this.amountOrdered--;
      this.$emit('orderedBurgers', {
         name: this.burger.name,
         amount: this.amountOrdered });
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
    width: 100%;
}
.button-container{
  display: flex;
  justify-content: space-between;
}
  </style>
  