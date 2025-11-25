<template>
<div>

<header>
        <img src="/background.jpg" alt="Background">
        <h1>Welcome to Burgermeister!</h1>
</header>

    <main>
        <section id="burgerselection"><br>
            <h2>Select your burger</h2>
            <p>Choose from our wide variety of burgers below.</p>
            <div class="burger-grid">
                <Burger v-for="burger in burgerMenu"
                    v-bind:burger="burger" 
                    v-bind:key="burger.name"
                    v-on:orderedBurger="addToOrder($event)"/>
                
            </div>
        </section>

        <section id="customerinformation"><br>
            <h2>Customer Information</h2>
            <p>Please indicate your delivery spot on the map and fill in your details below.</p>
            <h3>Delivery Information</h3>

        <div id="map-container">
            <img class="map" src="/img/polacks.jpg" alt="Map of Uppsala" @click="setLocation">

            <div v-if="location" class="dot" 
                v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">
                T
            </div>
        </div>
            
            <div class="customerInformationForm">
                <div>
                    <label for="fullname">Full name</label><br>
                    <input type="text" id="fullname" v-model="fn" required="required" placeholder="Full name">
                </div><br>

                <div>
                    <label for="email">E-mail</label><br>
                    <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                </div><br>

                <div>
                    <label for="paymentmethod">Payment method</label><br>
                    <select id="paymentmethod" v-model="rcp">
                        <option selected="selected">Mastercard</option>
                        <option>Visa</option>
                        <option>Amex</option>
                        <option>Swish</option>
                    </select>
                </div><br>
                <div>
                
                    <label for="choosegender">Choose your gender:</label><br>

                    <input type="radio" value="Male" v-model="cg" checked="checked">
                    <label for="male">Male</label><br>

                    <input type="radio" value="Female" v-model="cg">
                    <label for="female">Female</label><br>

                    <input type="radio" value="Do not wish to provide" v-model="cg">
                    <label for="Do not wish to provide">Do not wish to provide</label><br>

                </div>
                </div>

                <button class="submitButton" v-on:click="customerInfo">
                    <img src="/placeorder.jpg" alt="Place Order" width="20" height=20>
                        Place Order
                </button>

                

        </section>

    </main>

    <footer>
        <hr>
        <p>&copy; 2025 BurgerOnline. All rights reserved.</p>
    </footer>
    </div>


</template>

<script>
import Burger from '../components/OneBurger.vue'
import menu from '../assets/menu.json'
import io from 'socket.io-client'

const socket = io("localhost:3000");

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgerMenu: menu,
      
      fn: '',      // full name
      em: '',      // email
      rcp: 'Mastercard', // payment method 
      cg: 'Male',    // gender

      orderedBurgers:{

      },
      location: { x: 0, y: 0 }
    
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    
    
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

                     this.location.x = event.clientX - 10 - offset.x;
                     this.location.y = event.clientY - 10 - offset.y;

                    console.log("Location set to: ", this.location.x, this.location.y)
    },
    
    customerInfo: function(){
   
    console.clear();
    console.log('Customer Information:');
    console.log('Full name:', this.fn);
    console.log('Email:', this.em);
    console.log('Payment method:', this.rcp);
    console.log('Gender:', this.cg);
    console.log('Order:', this.orderedBurgers);

    socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: {x: this.location.x ,
                                          y: this.location.y,
                                          fullName: this.fn,
                                          email:this.em,
                                          paymentMethod: this.rcp,
                                          gender: this.cg
                              },
                                orderItems: {oI: this.orderedBurgers},
                                
                        }
                 );
                 
  },
  addToOrder: function (event) {
  this.orderedBurgers[event.name] = event.amount;
},
}
}
</script>

<style>

  @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

body {
    font-family: 'Times New Roman';
}

h1 {
    font-family: 'Agbalumo';
    font-size: 48pt;
}
h2 {
    font-family: 'Times new roman';
    font-size: 24pt;
}
h3 {
    font-family: 'Times new roman';
    font-size: 18pt;
}

main,
header {
    max-width: 100%;
}

main {
    font-family: 'Times New Roman';
    font-size: 14pt;
}

.ingredient {
    font-weight: bold;
}

section {
    
}

#burgerselection {
    background-color: black;
    color: white;
    border: 3px dashed white;
    margin: 0px 20px;
    padding: 20px;

}

#customerinformation {
    background-color: white;
    color: black;
    border: 3px dashed black;
    margin: 0px 20px;
    padding: 20px;

}
.customerInformationForm{
    border : 2px solid black;
    padding:10px;
}

button {
    margin: 10px;
    font-size: 14pt;
    font-family: 'Times New Roman';
}

button:hover {
    background-color: rgb(106, 251, 43);
}

.submitButton {
    margin-top: 30px;
    padding: 5px;
    
}


.addButton, .removeButton{
    padding: 5px 10px;
    

}
.removeButton:hover {
    background-color: rgb(251, 43, 43);
}

#burgerselection ul {
    list-style-type: disc;
    list-style-position: inside;
}

.burger-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));

}

.burger {
    margin: 10px;
    padding: 50px;
    background-color: rgb(21, 21, 21);
    border-radius: 20px;

}

header {
    margin: 0px 20px;
    height: 300px;
    overflow: hidden;

}

header img {

    opacity: 0.5;
    width: 100%;
    height: auto
}


header h1 {
    position: absolute;
    margin-top: -520px;
    margin-left: 300px;

}
footer{
    font-family: 'Times New Roman';
    font-size: 14pt;
    font-weight: bold;
}

#map-container {
  position: relative;
  overflow:scroll;
  width: 100%;
  height: 600px
}

.map {
    display: block;  
}

.dot {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

</style>