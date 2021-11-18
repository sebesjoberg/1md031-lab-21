<template>



  <div>
  <section class="burgers">

    <div v-bind:class="burger.number"> <h5 class="burgernames">{{ burger.name }}</h5>
      <img v-bind:src="burger.URL"
      v-bind:title="burger.name"  width="400" height="300">
      <ul>

        <section class="innehåll" v-if="burger.gluten"><li>Innehåller gluten</li>
        </section>

        <section class="innehåll" v-if="burger.lactose"><li>Innehåller laktos</li>
        </section>
        <section class="innehåll" v-if="burger.nyttig"><li>OBS! nyttig</li>
        </section>
        </ul>
       <section class="ordered">Ordered:{{ amountOrdered}}</section>

       <button v-on:click="minusBurger" class="minus">
         -1
       </button>
       <button v-on:click="addBurger" class="add">
         +1
       </button>
      </div>
</section>
    </div>

</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
  return {
    amountOrdered: 0,
  }
},
methods:{
  minusBurger: function () {

  this.amountOrdered -= 1;
  if(this.amountOrdered<0){
    this.amountOrdered=0;
  }

  this.$emit('orderedBurger', { name:   this.burger.codename,
                                amount: this.amountOrdered
                              }
  );

},
  addBurger: function () {

  this.amountOrdered += 1;
  this.$emit('orderedBurger', { name:   this.burger.name,
                                amount: this.amountOrdered
                              }
  );


}


}

}





</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ordered{

  margin-left:110px;
}
button{
  margin-left:90px;
  width: 40px;
  height: 40px;
}
.minus{
  background-color: red;
}
.add{
  background-color: green;
}
</style>
