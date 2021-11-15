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


        </ul>

      </div>
</section>
    </div>

</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  }}





</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
