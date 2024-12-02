

<template>

<header class="header">
            <img src="/img/Ny-bakgrund.jpeg">
            
            <h1>Saras Burgers</h1>
            
        </header>
        <main>
            <section>
                <h2>Välj din burgare</h2>
                <p>Här kan du välja bland våra hamburgare</p>
                <div class="burger-grid">
                    <Burger
                    v-for="burger in burgers"
                    :key="burger.name"
                    :burger="burger"
                    v-on:orderedBurger="addToOrder"
                    />
                </div>
            </section>
            <section class="Kontaktformulär" id="Kontaktinformation">
                <h2>Kontaktinformation</h2>
                <p>Fyll in kontaktuppgifter</p>
                <form>
                    <h3>Namn: </h3>
                    <input v-model="fullname" placeholder="För- och efternamn" />                    
                    <h3>E-mail:</h3>
                    <input v-model="email" placeholder="E-mail adress" />
                    <h3>Betalsätt:</h3>
                    <select v-model="betalsätt">
                        <option disabled value="">Välj en</option>
                        <option>Kort</option>
                        <option>Faktura</option>
                        <option>Presentkort</option>
                    </select>
                    <h3>Kön:</h3>
                    <div>
                    <input type="radio" id="man" value="Man" v-model="kön" />
                    <label for="man">Man</label>
                    </div>
                    <div>
                    <input type="radio" id="kvinna" value="Kvinna" v-model="kön" />
                    <label for="kvinna">Kvinna</label>
                    </div>
                    <div>
                    <input type="radio" id="ickebinär" value="Ickebinär" v-model="kön" />
                    <label for="ickebinär">Ickebinär</label>
                    </div>
                    <div>
                    <input type="radio" id="villInteAnge" value="Vill inte ange" v-model="kön" />
                    <label for="villInteAnge">Vill inte ange</label>
                    </div>
                </form>
            <h3>Vänligen ange leveransadress:</h3>
            <div style="width: 500px; height: 400px; overflow: scroll; border: 1px solid">
            <div id="map" v-on:click="setLocation">
                <div 
                v-bind:style="{left: location.x + 'px', top: location.y + 'px'}"
                style="position: absolute; 
                font-weight: bold;
                background-color: black;
                color: white;
                border-radius: 50%;
                width: 20px;
                height: 20px;
                display: flex;
                justify-content: center;
                align-items: center;">
            T
            </div>
        </div>
    </div>
    <button type="submit" v-on:click="submitForm">
                    <img class="buttonimg" src="/img/Delivery.jpeg">
                    Skicka Beställning
                  </button>
            </section>
        </main>
        <footer>
            @Burger2024
        </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function menuItem(name, image, kCal, gluten, lactose) {
  this.name = name;
  this.image = image;
  this.kCal = kCal;
  this.gluten = gluten;
  this.lactose = lactose;
}

/*const burgers = [
  new menuItem("Klassikern", "/img/Klassikern.jpeg", 600, true, true),
  new menuItem("Krispet", "/img/Kyckling-burgare.jpeg", 550, false, true),
  new menuItem("Grönt är skönt", "/img/Vegan-nr2.jpeg", 450, false, false)
];*/

/*console.log(burgers);*/

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      orderedBurgers:{},
      fullname: "",
      email: "",
      betalsätt: "",
      kön: "",
      amountOrdered: 0,
      location: {x: 0, y: 0}
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
        this.location = {
            x: event.clientX - offset.x,
            y: event.clientY - offset.y
        };
        console.log("Updated location:", this.location);
    },
    addToOrder(event){
        this.orderedBurgers[event.name]=event.amount;
        console.log(this.orderedBurgers);
    },
    submitForm: function () {
        const orderData = {
            orderId: this.getOrderNumber(),
            details: this.location,
            orderItems: this.orderedBurgers,
            customerInfo: {
                name: this.fullname,
                email: this.email,
                paymentMethod: this.betalsätt,
                gender: this.kön,
            }
        };
        socket.emit("addOrder", orderData);
        console.log("Order sent:", orderData);
    }
  }
}
</script>

<style>
  #map {
    width: 1920px;
    height: 1028px;
    background: url("/img/polacks.jpg");
    background-size: cover;
    background-repeat: no-repeat;
    position: relative;
  }
  /*@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');*/
@import url('https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400..700;1,400..700&family=Playfair+Display:ital,wght@0,400..900;1,400..900&display=swap');
body {
    font-family:'Lora', serif;
    font-size: 12pt;
}

/*p {
    color: red;
}*/

h1 {
    font-family: 'Agbalumo';
    font-size: 36pt;
}
main, header, footer, nav ul {
    max-width: 80rem;
    margin: 0 auto 0 auto;
}
main {
    background-color: bisque;
}

/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */

.header {
    position:relative;

    /*background-size: cover;*/
    overflow: hidden;
    width: 100%;
    height: 200px;
    margin: 20px auto;
}


.header img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    /*margin-top: -20px;*/
    z-index: -1;
    opacity: 0.7;

}

.header h1 {
    /*width:40rem;*/
    /*margin: 50px auto;*/
    text-align: center;
    line-height: 200px;
    color: #be1a1a;
    text-shadow: 2px 2px 2px black;
    margin: 0;
}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: grey;
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

.Allergi{
    font-weight: bold;
}

/*ul li:first-of-type {
    color:red;
}*/

.Kontaktformulär {
    background-color: black;
    color: white;
}

button:hover {
    background-color:blue;
    color: white;
    cursor: pointer;

 }
 .buttonimg{
    width: 17px;
    height: 17px;
 }

section{
    margin:1px;
    margin-top: 10px;
    border: 2px dashed currentColor;
    border-radius: 5px;
    padding: 10px
 }

 .burger-grid {
    display: grid;
    grid-template-columns: 33% 33% 33%;
    grid-gap: 5px;
 }

.burger {
    margin: 5px; /* Lägg till lite mellanrum mellan dem */
    padding: 5px;
    border: 2px solid hsl(297, 72%, 87%);
    border-radius: 5px;
}
.burger img{
    width:100%;
    height: auto;
}
</style>