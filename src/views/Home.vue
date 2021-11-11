<template>
  <div>
    Burgers
    <Burger v-for="burger in burgers"
            v-bind:burger="burger"
            v-bind:key="burger.name"/>
  </div>
  <div id="map" v-on:click="addOrder">
    click here
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
console.log(Items);



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
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>
