<template>
    <div>
      {{ burger.name }} {{ burger.kCal }}
    </div>
  </template>
  
  <script>

  function MenuItem(name, imageUrl, kCal, containsGluten, containsLactose) {
    this.productName = name;
    this.imageUrl = imageUrl;
    this.kCal = kCal;
    this.containsGluten = containsGluten;
    this.containsLactose = containsLactose;
}

  const burgers = [];
  
  const BBQBurger = new MenuItem("BBQ Burger", "https://assets.tmecosys.com/image/upload/t_web767x639/img/recipe/ras/Assets/102cf51c-9220-4278-8b63-2b9611ad275e/Derivates/3831dbe2-352e-4409-a2e2-fc87d11cab0a.jpg", 650, true, true );
  const meatLover = new MenuItem("Meat Lover", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQNfbgZdQMbfIP2v7MWpYKDn2kMIUsVzlw8rg&usqp=CAU", 2500, true, true );
  const veganHeaven = new MenuItem("Vegan Heaven", "https://www.inspiredtaste.net/wp-content/uploads/2018/05/Homemade-Mushroom-Veggie-Burger-Recipe-1200.jpg", 0, false, false);

  burgers.push(BBQBurger, meatLover, veganHeaven);
  console.log(burgers)
  
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
  </style>
  