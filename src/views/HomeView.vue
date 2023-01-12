<template>
  <div>
    <header class="header">
            <img id="headimg" src="https://images.affordableseating.net/dpr_1.0,f_auto,q_auto/afd/media/contenttype/restaurant_bar_-_header.jpg"/>
            <h1 id="textpos">Welcome to SuperSmash online</h1>  
    </header>

    <div id="menu"> 
      <h4 class="box d">Select burgers</h4>
      <p class="box e">This is where you select burgers</p>

      <div class="wrapper">
        <Burger v-for="burger in burgers"
                  v-bind:burger="burger" 
                  v-bind:key="burger.name"
                  v-on:orderedBurger="addToOrder($event)"/>
      </div>
    
    </div>
    
        <section id="info">
            <h4>Customer information</h4>
            <p>This is where you provide necessary information</p>
            <h4>Delivery information</h4>

                <p>
                    <label for="fullname">Full name</label><br>
                    <input type="text" id="fullname" v-model="Name" required="required" placeholder="First- and Last name">
                </p>
                <p>
                    <label for="email">E-mail</label><br>
                    <input type="text" id="email" v-model="Email" required="required" placeholder="E-mail address">
                </p>
              <!--  <p>
                    <label for="street">Street</label><br>
                    <input type="text" id="street" v-model="Street" required="required" placeholder="Street name">
                </p>
                <p>
                    <label for="number">House number</label><br>
                    <input type="number" id="number" v-model="Streetnumber" required="required" placeholder="House number">
                </p>-->
            <p>
                <label for="payment">Payment method</label><br>
                <select id="payment" v-model="pym">
                    <option selected="selected">Credit card</option>
                    <option>Cash when deliverd</option>
                    <option>Klarna</option>
                    <option>Swish</option>
                </select>
             </p>
                <legend>Gender</legend>

                <div>

                <div>
                    <input type="radio" id="woman" v-model="gender" value="woman" checked>
                    <label for="woman">Female</label>
                </div>

                <div>
                    <input type="radio" id="man" v-model="gender" value="man" checked>
                    <label for="man">Male</label>
                </div>

                <div>
                    <input type="radio" id="nonbinary" v-model="gender" value="nonbinary" checked>
                    <label for="nonbinary">Non-binary</label>
                </div>

                <div>
                    <input type="radio" id="und" v-model="gender" value="und" checked>
                    <label for="und">Undisclosed</label>
                </div>  
              </div>           
        </section>

    <button v-on:click="order" type="submit" class="button">
        <img src="https://image.shutterstock.com/image-vector/fast-shipping-delivery-truck-flat-260nw-353170508.jpg"
        width="20"
        height="15">
        Place my order!
      </button>
    <hr>
    <footer>
        <p> &copy; 2022 Super Smash inc</p>
    </footer>

    <!--Här var det en div med burger-loopen-->
    <div id="scroller">
      <div id="map" v-on:click="setLocation">
        <div v-bind:style="{left: location.x+'px', 
                            top: location.y+'px'}">
          T
        </div>
    </div>
    </div>
  </div>
    
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

//function MenuItem(burgerName, img, kCal, info, gluten, lactose) {
  //  this.name = burgerName; // The *this* keyword refers to the object itself
  //  this.img = img;
  //  this.kCal = kCal;
  //  this.info = info;
  //  this.gluten = gluten;
  //  this.lactose = lactose;
  //  }

// Objects are then instantiated using the *new* keyword
//const burg1 = new MenuItem('Super Smash burger', "https://i.pinimg.com/originals/8f/41/b2/8f41b2d2b12791088da03500bd5cf909.jpg", '600 kCal', 'Cheese, pickled onion, tomato, salad and house dressing' , 'gluten', 'lactose');
//const burg2 = new MenuItem('Vegan bean burger', "https://ordinaryvegan.net/wp-content/uploads/2017/11/Vegan-Black-Bean-Burger-with-Beets-and-Quinoa.jpg", '450 kCal', 'Walnut bread, bean pattie, tomato, avocado and vegan mayonnaise', 'gluten', 'No lactose');
//const burg3 = new MenuItem('Crispy Chicken burger', "https://www.kitchensanctuary.com/wp-content/uploads/2019/08/Crispy-Chicken-Burger-square-FS-4518.jpg", '550 kCal', 'Fried chicken, herb mayonnaise, tomato and pickled onion', 'gluten', 'lactose');

//const burger = [burg1, burg2, burg3];

//console.log( burg1.burgerName ); 

export default {
  name: 'HomeView',
  components: {
    Burger
  },

  data: function () {
    return {

         burgers: menu, 
         Name:'',
         Email:'',
         //Street:'',
         //Streetnumber:'',
         pym:'',
         gender:'',
         orderedBurgers:{},
         location: {x:0, y:0}
    }

  },
  methods: {

    order: function(){
      console.log(this.Name, this.Email, this.gender, this.pym);
      console.log(this.orderedBurgers);
      socket.emit("addOrder", {orderId: this.getOrderNumber(), 
        details: {
          x: this.location.x,
          y: this.location.y
      },

      orderItems: this.orderedBurgers,
      customerInformation: [this.Name, this.Email, this.pym, this.gender]
    });

    //Tog bort this.Street, this.Streetnumber

  },

  setLocation: function (event){
    this.location.x=event.clientX - 10 - event.currentTarget.getBoundingClientRect().left;
    this.location.y=event.clientY - 10 - event.currentTarget.getBoundingClientRect().top;
  },

  printOrder: function(){
    console.log(this.Name, this.Email, this.gender, this.pym);
    console.log(this.orderedBurgers);
  },

  addToOrder: function(event){
    this.orderedBurgers[event.name]=event.amount;
  },

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
                 this.location.x=event.clientX - 10 - offset.x;
                 this.location.y=event.clientY - 10 - offset.y;
    }
  }

}
</script>

<style>
  #map {
    width: 1920px;
    height: 1078px;
    background: url("http://lh4.ggpht.com/-nm71sW2ziq8/TgoGTE1ZItI/AAAAAAAADAI/Ol5uVs-mmzM/karta_uppsala_osm.jpg?imgmax=800");
    background-size: 1920px, 1078px;
    position:absolute;
    background-repeat: no-repeat;
  }

  #scroller{
    overflow: scroll;
    position:relative;
    width: 1300px;
    height: 500px;
  }

  #map div{ 
    position:absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width: 20px;
    height: 20px;
    text-align: center;
  }

  body {
    font-family: 'Times New Roman', sans-serif;
    font-style: normal;
    font-size: 12pt;
    background-color: rgb(209, 205, 199);
}

@media screen and (max-width: 800px) {
    h1 {
        font-size: 6vw;
    }
}

a, strong, em {
    color: #0099ff;
}

p.info,
#login p {
    width: 80%;
    margin: 1vw auto;
}

#menu {
    background-color: black;
    color: white; 
    padding: 25px;
    margin: 20px;
    margin-left: 30px;
    border: dotted white;
}

#info{
    background-color: white;
    padding: 25px;
    margin: 20px;
    margin-left: 30px;
    border: dotted black;
}

#al {
    font-weight: bold;
}

input[type="button"] {
    height: .7em;
    width: .7em;
}

button:hover {
    background-color: green;
 }
 .button{
    margin: 15px;
    margin-left: 30px;
 }

 #textpos{
    text-align: center;
    padding: 40px;
 }

 .header{
    padding: 10px;
    height: 200px;
    overflow: hidden;
    position:relative;
 }

 #headimg{
    opacity: 0.5;
    height: auto;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -100
 }

 .wrapper{
    display: grid;
    grid-template-columns: 300px 300px 300px;
 }

 .box{
    border-radius: 800px;
    padding: 10px;
    font-size: 100%;
 }

.d{
    grid-column: 1;
    grid-row: 1;
    margin-top: 10px;
}

.e{
    grid-column: 1;
    grid-row: 1;
    margin-top: 50px;
}

.burgerrow{
  grid-row: 2;
  grid-row: 2;
  grid-template-rows: auto;
  grid-auto-flow: row;
  align-items: strech;
  }


</style>





            <!--<div class="box a">
            <h4>SUPER SMASH BURGER</h4>
            <img src="https://i.pinimg.com/originals/8f/41/b2/8f41b2d2b12791088da03500bd5cf909.jpg"
            width="260"
            height="250"/>
            <ul class="allergies">
                <li>Cheese, pickled onion, tomato, salad and house dressing</li>
                <li>Contains <span id="al">gluten</span></li>
                <li>Contains <span id="al">lactose</span></li>
            </ul>
        </div>-->
        <!--<div class="box b">
            <h4>VEGAN BEAN BURGER</h4>
            <img src="https://ordinaryvegan.net/wp-content/uploads/2017/11/Vegan-Black-Bean-Burger-with-Beets-and-Quinoa.jpg"
            width="270"
            height="250"/>
            <ul class="allergies">
                <li>Walnut bread, bean pattie, tomato, avocado and vegan mayonnaise</li>
                <li>Contains <span id="al">gluten</span></li>
                <li>Contains <span id="al">nuts</span></li>
            </ul>
        </div>-->
        <!--<div class="box c">
            <h4>CRISPY CHICKEN BURGER</h4>
            <img src="https://www.kitchensanctuary.com/wp-content/uploads/2019/08/Crispy-Chicken-Burger-square-FS-4518.jpg"
            width="250"
            height="250"/>
            <ul class="allergies">
                <li>Fried chicken, herb mayonnaise, tomato and pickled onion</li>
                <li>Contains <span id="al">gluten</span></li>
                <li>Contains <span id="al">egg</span></li>
            </ul>
        </div>-->