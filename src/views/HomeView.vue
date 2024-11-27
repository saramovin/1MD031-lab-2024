

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
                    />
                </div>
            </section>
            <section class="Kontaktformulär" id="Kontaktinformation">
                <h2>Kontaktinformation</h2>
                <p>Fyll in kontaktuppgifter</p>
                <form>
                    <p>Full name: {{ fullname }} </p>
                    <input v-model="fullname" placeholder="First- and lastname" />                    
                    <p>E-mail: {{ email }} </p>
                    <input v-model="email" placeholder="E-mail adress" />
                    <p>Street: {{ streetname }} </p>
                    <input v-model="streetname" placeholder="Streetname" />
                    <p>House number: {{ housenumber }} </p>
                    <input v-model.number="housenumber" placeholder="House number" />
                    <div>Betalsätt: {{ betalsätt }}</div>
                    <select v-model="betalsätt">
                        <option disabled value="">Please select one</option>
                        <option>Kort</option>
                        <option>Faktura</option>
                        <option>Presentkort</option>
                    </select>
                    <div>Kön: {{ kön }}</div>
                    <input type="radio" id="man" value="Man" v-model="kön" />
                    <label for="man">Man</label>

                    <input type="radio" id="kvinna" value="Kvinna" v-model="kön" />
                    <label for="kvinna">Kvinna</label>

                    <input type="radio" id="ickebinär" value="Ickebinär" v-model="kön" />
                    <label for="ickebinär">Ickebinär</label>

                    <input type="radio" id="villInteAnge" value="Vill inte ange" v-model="kön" />
                    <label for="villInteAnge">Vill inte ange</label>
                </form>
                <button type="submit" v-on:click="submitForm">
                    <img class="buttonimg" src="/img/Delivery.jpeg">
                    Send Order
                  </button>
            </section>

            <div id="map" v-on:click="addOrder">
    click here
    </div>

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
      fullname: "",
      email: "",
      streetname: "",
      housenumber: null,
      betalsätt: "",
      kön: ""
    }
  },
  methods: {
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
    },
    submitForm: function () {
        console.log("Full name:", this.fullname);
        console.log("Email:", this.email);
        console.log("Street:", this.streetname);
        console.log("House number:", this.housenumber);
        console.log("Betalsätt:", this.betalsätt);
        console.log("Kön:", this.kön);
    }
  }
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
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