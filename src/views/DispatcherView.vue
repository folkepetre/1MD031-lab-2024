<template>
    <div id="orders">
      <div id="orderList">
        <div v-for="(order, key) in orders" v-bind:key="'order'+key">
          #{{ key }}: 
          <div v-for="items in order.orderItems">
            <span v-for="(amount, name) in items" :key="name">
              {{ name }}: {{ amount }},
            </span>
            
          </div>
          <div class="orderDetails">
            {{ order.details.fullName }}
            ({{ order.details.email }},
            {{ order.details.paymentMethod }},
            {{ order.details.gender }})
          </div>
          <hr>
        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots">
        <div v-for="(order, key) in orders" 
          v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dot'+key"
          >
          T
        </div>
      </div>
      
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io("localhost:3000");
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null,
        location: { x: 0,
            y: 0
          }
      }
      

    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      },
      changeStatus: function(orderId) {
        socket.emit('changeStatus', {orderId: orderId, status: "Annan status"});
      },
      updateLocation: function(event) {
        var offset = {x: event.currentTarget.getBoundingClientRect().left,
                      y: event.currentTarget.getBoundingClientRect().top};
        this.location.x = event.clientX - 10 - offset.x;
        this.location.y = event.clientY - 10 - offset.y;  
      
    }
  }
}
  </script>
  <style>
  #orderList {
    top:1em;
    left:1em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(255,255,255, 0.5);
    padding: 1em;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
    background-image: url('/img/polacks.jpg');
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  .orderDetails{
    font-style: italic;
    font-size: 11pt;
  }
  

  </style>
