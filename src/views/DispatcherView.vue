<template>
  <div id="orders">
    <div id="orderList">
      <div class="oneOrder" v-for="(order, key) in orders" v-bind:key="'order' + key">
        <span style="font-size: large; font-weight: bold;"> Order no. {{ key }} </span><br>
        <span style="font-size: medium; font-weight: normal;"> Customer Information:</span> <br>
        {{ order.customer.name }} - {{ order.customer.email }} <br>
        Payment: {{ order.customer.paymentMethod }}, Gender:  {{ order.customer.gender }}
        <div class="borgir" v-for="(quantity, burger) in order.details.orderItems" v-bind:key="burger">
          <p v-if="quantity > 0">{{ burger }}: {{ quantity }}</p>
        </div>
        <hr>
      </div>
      <button v-on:click="clearQueue">Clear Queue</button>
    </div>
    <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg') + ')' }">
      <div v-for="(order, key) in orders"
        v-bind:style="{ left: order.details.location.x + 'px', top: order.details.location.y + 'px' }"
        v-bind:key="'dots' + key">
        {{ key }}
      </div>
    </div>
  </div>
</template>
  
<script>
import io from 'socket.io-client'
const socket = io();

export default {
  name: 'DispatcherView',
  data: function () {
    return {
      orders: null,
    }
  },
  created: function () {
    socket.on('currentQueue', (data) => {
      this.orders = data.orders;
      console.log(this.orders)
    });
  },
  methods: {
    clearQueue: function () {
      socket.emit('clearQueue');
    }
  }
}
</script>
<style>
#orderList {
  top: 1em;
  left: 1em;
  position: absolute;
  z-index: 2;
  color: black;
  background: rgba(255, 255, 255, 0.5);
  padding: 1px;
  margin: 0;
}

#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  width: 1920px;
  height: 1078px;
  cursor: crosshair;
}

#dots div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}

.borgir {
  margin: 0;
  padding: 0;
}

.oneOrder {
  margin: 25px;
  padding: 0;
}
</style>
  