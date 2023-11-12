<template>
  <div class="box">
    <h3>{{ burger.name }}</h3>
    <img v-bind:src="burger.img" alt="Burger Image" title="burger.name" style="width: 200px;">
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
  