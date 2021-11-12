<template>
  <header>
    <h1>Välkommen till BurgerOnline</h1>

  </header>


<main>
  <section class="choose">
    <h3>Välj Hamburgare</h3>
    <div>Här väljer du vad du ska beställa</div>
  </section>
  <section class="burgers">
    <div class="burger 1"> <h5 class="burgernames">The Bro Burger</h5>
      <img src="https://cdn.shopify.com/s/files/1/0372/9054/1115/products/hampshirehamburgarekopiera_1200x1200.jpg?v=1627649844"
      title="Bro Burger"  width="400" height="300">
      <ul>
        <section class="innehåll"><li>Innehåller gluten</li>
        </section>
        <section class="innehåll"><li>Innehåller laktos</li>
        </section>
        <section class="innehåll"> <li> OBS! stark</li>
        </section>
        </ul>
      </div>
      <div class="burger 2"> <h5 class="burgernames">Fullkornsburgaren</h5>
        <img src="http://skippanudlarna.files.wordpress.com/2012/05/egna-hamburgarestorbild.jpg"
        title="Fullkornsburgaren" width="400" height="300">
        <ul>
          <section class="innehåll">  <li>Innehåller gluten</li>
          </section>
          <section class="innehåll"><li>  OBS! nyttig</li>
          </section>
        </ul>
        </div>
        <div class="burger 3"> <h5 class="burgernames">Klassikern</h5>
          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRa38lJgnL3-bs2NQhc2pOdPsjyQSKoP6CiGA&usqp=CAU"
          title="Klassikern" width="400" height="300">
          <ul>
            <section class="innehåll">  <li>Innehåller gluten</li>
            </section>
            <section class="innehåll">  <li>Innehåller laktos</li>
            </section>
            </ul>
            </div>
          </section>


        <section class="information" >
          <div  style="Clear:left"><h3>Personlig information</h3></div>
          Ange nödvändig information
          <h4>Leverans information</h4>
<form>
          <p>

              <label for="För och efternamn">För och efternamn</label><br>
              <input type="text" id="för och efternamn" name="förocheft" required="required" placeholder="För och efternamn">
            </p>
            <p>
              <label for="E-Mail adress">E-Mail adress</label><br>
              <input type="email" id="E-Mail" name="E-Mail" required="required" placeholder="E-mail adress">
            </p>
            <p>
              <label for="gata">Gatunamn</label><br>
                <input type="text" id="Gata" name="Gata" required="required" placeholder="Gatunamn">
              </p>
              <p>
                <label for="Hus nummer">Hus nummer</label><br>
                <input type="number" id="Hus nummer" name="Hn" required="required" placeholder="Hus nummer">
              </p>
              <div style=float:left><label for="man">Man</label><br></div>
              <div ><input type="radio" id="Man" name="Kön"></div>
              <div style=float:left><label for="Kvinna">Kvinna</label><br></div>
              <div ><input type="radio" id="Kvinna" name="Kön"></div>
              <div style=float:left><label for="Icke Binär">Icke Binär</label><br></div>
              <div ><input type="radio" id="Icke Binär" name="Kön"></div>
              <div style=float:left><label for="Villej">Vill ej uppge</label><br></div>
              <div ><input type="radio" id="Villej" name="Kön" checked="checked"></div>
              <button type="submit">
                <img src="https://previews.123rf.com/images/sarahdesign/sarahdesign1503/sarahdesign150300752/37543106-ok-button.jpg"
                height="40">
                Place Order!
              </button>
            </form>
          </section>
        </main>

        <Footer>
          <hr>
          &copy; 2021 Fakeburgers AB
        </Footer>
        <div>

          <Burger v-for="burger in burgers"
          v-bind:burger="burger"
          v-bind:key="burger.name"
          v-bind:key2="burger.KCal"/>
  </div>


</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
const socket = io();

const Items = [
  new MenuItem('The Bro Burger','https://cdn.shopify.com/s/files/1/0372/9054/1115/products/hampshirehamburgarekopiera_1200x1200.jpg?v=1627649844'
  , 1000, 'true', 'true'),new MenuItem('Fullkornsburgaren','http://skippanudlarna.files.wordpress.com/2012/05/egna-hamburgarestorbild.jpg'
  , 500, true, false), new MenuItem('Klassikern','https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRa38lJgnL3-bs2NQhc2pOdPsjyQSKoP6CiGA&usqp=CAU',
  1500,true,true)
]

function MenuItem(name, URL, KCal, Gluten, Lactose){
  this.name=name;
  this.URL=URL;
  this.Gluten=Boolean(Gluten);
  this.Lactose=Boolean(Lactose);
}

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: Items
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
    }
  }
}
</script>

<style>
  @import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';

  body {
     font-family: "cambria", serif;
     font-style: italic;
   }
   .innehåll {
     text-transform: uppercase;
     font-weight: bold;
  }

  .burgernames{
    margin-left: 50px;
  }
  .headline header{
    position:absolute;
    margin-top: -1100px;
    margin-left: 500px;
  }
  .headline img{
    opacity: 0.6;
    width: 100%;
    height:auto;
  }
  .headline{
    margin:10px 10px 10px 10px;
    height: 100px;
    overflow:hidden;
  }


  .information{
    margin: 10px 10px 10px 10px;
  border: 2px dashed black;
   padding-left:10px;
  }


  button{
    margin-top: 20px;
    margin-bottom: 10px;
  }
  .choose{
    margin:10px;
  }
  button:hover {
    background-color: green;
    cursor: pointer;

  }
  .burgers {
       display: grid;
       grid-gap: 10px;
       margin-left: 10px;
       grid-template-columns: 415px 415px 415px ;
      background-color: black;
      color: white;
      padding-top: 10px;
      padding-bottom: 10px;


   }
   .burger {
     font-size: 150%;
    border: 2px dashed white;
     margin-left: 10px;
     background-color: black;
   }
   @media screen and (max-width: 800px) {
      h1 {
          font-size: 6vw;
      }
  }





  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>
