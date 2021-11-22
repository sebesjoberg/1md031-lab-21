<template>
<div>
  <section class="headline">
    <img src="https://modernalivet.se/wp-content/uploads/2020/10/q-u-i-S6atLH5Rf0U-unsplash.jpg"
    title="headliner">

    <div><h1>BurgerOnline</h1></div>

  </section>
  </div>
  <header>
    <h1>Välkommen till BurgerOnline</h1>

  </header>


<main>
  <section class="choose">
    <h3>Välj Hamburgare</h3>
    <div>Här väljer du vad du ska beställa</div>
  </section>
  <section class="burgers">
  <Burger v-for="burger in burgers"
          v-bind:burger="burger"
          v-bind:key="burger.name"
          v-on:orderedBurger="addToOrder($event)"/>

            </section>


        <section class="information" >
          <div  style="Clear:left"><h3>Personlig information</h3></div>
          Ange nödvändig information
          <h4>Leverans information</h4>
<form>
          <p>

              <label for="För och efternamn">För och efternamn</label><br>
              <input v-model="namn" type="text" id="form namn" required="required" placeholder="För och efternamn">
            </p>

            <p>
              <label for="E-Mail adress">E-Mail adress</label><br>
              <input v-model="mail" type="email" id="form mail"  required="required" placeholder="E-mail adress">
            </p>



              <div id="kön">
              <select v-model="kön" required="required">
              <option disabled value="">Ange kön</option>
               <option>Vill ej ange</option>
                <option>Kvinna</option>
              <option>Man</option>
              <option>Icke binär</option>
              </select>
              </div>
              <div id="pay">
              <select v-model="pay" required="required">
              <option disabled value="">Välj betalmetod</option>
               <option>Kort</option>
                <option>Paypal</option>
              <option>Klarna</option>
              <option>Swish</option>
              </select>
              </div>


<label>Ange plats</label>
<div id="wrap">
<div id="map" v-on:click="setLocation"></div>
<div id="loc" v-bind:style="{ left: location.x + 'px',
            top: location.y + 'px' }">
            T
        </div></div>
              <button v-on:click="submiter" type="submit">
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


  </div>

</template>

<script>
import Burger from '../components/Burger.vue'
import menu from '../assets/menu.json'
import io from 'socket.io-client'
const socket = io();

/* function MenuItem(name, URL, KCal, Gluten, Lactose, number){
  this.name=name;
  this.URL=URL;
  this.Gluten=Boolean(Gluten);
  this.Lactose=Boolean(Lactose);
  this.number="burger"+" "+number;}
   */




export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      namn: "",
      mail:"",
      kön:"",
      orderedBurgers:{},
      location:{x:0, y:0},
      pay:""

    }
  },
   methods: {
     addToOrder: function (event) {

this.orderedBurgers[event.name] = event.amount;


}, setLocation: function(event){
const offset=event.target.getBoundingClientRect();
this.location.x = event.clientX-offset.left-10;
this.location.y = event.clientY-offset.top-10;
},
    submiter: function (){
     const form = /\S+@\S+\.\S+/;

      if(this.namn!=""  && this.kön!="" &&
      form.test(this.mail) && this.pay!=""){
      var orderId=this.getOrderNumber();
      alert("order was sent! Order number is:"+orderId);
      console.log(this.orderedBurgers)
      socket.emit("addOrder",{ orderId,
                                details: { x: this.location.x,
                                           y: this.location.y },
                                orderItems: this.orderedBurgers ,
                              personalInfo:{ name: this.namn,
                                email: this.mail,
                                gender: this.kön,
                                payment: this.pay}

                              });  }
      else{
        alert("Your order was not complete, please fill it out correctly")
      }
    },


    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },


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
  .headline div{
    position:absolute;
    margin-top: -1100px;
    margin-left: 500px;
    font-family: "cambria", serif;
    font-style: italic;

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
#wrap{
  width:800px;
     height:500px;
  overflow:scroll;
 position: relative;

}
#loc{

  background-color: black;
  color: white;
text-align: center;
  position: absolute;
  width:20px;
  height:20px;
  border-radius: 10px;

}
  #map {


    background: url("/img/polacks.jpg");
     width:1920px;
        height:1078px;

  }
</style>
