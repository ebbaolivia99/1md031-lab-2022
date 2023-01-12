<template>
    <div id="orders">
      <div id="orderList">
        <div v-for="(order, key) in orders" v-bind:key="'order'+key"> 

        #{{ key }}: {{ order.orderItems }}
        <dd>{{order.customerInformation}}</dd>

        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots">
          <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
            {{ key }}
          </div>
      </div>
      <!--v-bind:style="{ background: 'scr(' + require('http://lh4.ggpht.com/-nm71sW2ziq8/TgoGTE1ZItI/AAAAAAAADAI/Ol5uVs-mmzM/karta_uppsala_osm.jpg?imgmax=800')+ ')' }-->
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io();
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
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
    background: url("http://lh4.ggpht.com/-nm71sW2ziq8/TgoGTE1ZItI/AAAAAAAADAI/Ol5uVs-mmzM/karta_uppsala_osm.jpg?imgmax=800");
    background-size: 1920px, 1078px;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 40px;
    width:50px;
    height:50px;
    text-align: center;
  }
  </style>
  