<template>

    <div class="burgerrow">
      <div class="box">
            <h4>{{burger.name}}</h4>
            <img v-bind:src="burger.img" style="width: 220px; height:200px" />
            <ul class="allergies">
                <li>{{burger.kCal}} kCal</li>
                <li>{{burger.info}}</li>
                <li id="al">{{burger.gluten ? "gluten" : "no gluten"}}</li>
                <li id="al">{{burger.lactose ? "lactose" : "no lactose"}}</li>
            </ul>
        </div>

        <div>
          <button v-on:click="increase">+</button>
          {{amountOrdered}}
          <button v-on:click="decrease">-</button>
        </div>

    </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    data:function(){
      return{
        amountOrdered: 0
      }
    },
  

  methods:{
    increase: function(){
    this.amountOrdered +=1;
    this.$emit('orderedBurger', {name: this.burger.name, amount: this.amountOrdered});
  },

    decrease: function(){
    if(this.amountOrdered>0){
    this.amountOrdered -=1
    this.$emit('orderedBurger', {name: this.burger.name, amount: this.amountOrdered});
  }}

  }}
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

  box{
    border-radius: 800px;
    padding: 10px;
    font-size: 100%;
    grid-row: 2;
  }

  #al {
    font-weight: bold;
  }

  .burgerrow{
    grid-row: 2;
    grid-template-rows: auto;
    grid-auto-flow: row;
    align-items: strech;
  }
  
  </style>
  