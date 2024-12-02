<template>
  <div class="burger">
    <h3>{{ burger.name }}</h3>
    <img v-bind:src="burger.url">
    <ul>
      <li>{{ burger.kCal }} kalorier</li>
      <li v-if="burger.gluten" class="Allergi">Innehåller gluten</li>
      <li v-if="burger.lactose" class="Allergi">Innehåller laktos</li>
      <li v-if="!burger.lactose && !burger.gluten">Gluten- och laktosfri</li>
    </ul>
    <p>Antal: <button @click="decreaseOrder">-</button> {{ amountOrdered }} <button @click="increaseOrder">+</button></p>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object,
  },
  data: function() {
    return {
      amountOrdered: 0
    }
  },
  methods: {
    increaseOrder () {
      this.amountOrdered++;
      this.emitOrder();
    },
    decreaseOrder () {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.emitOrder();
      }
    },
    emitOrder() {
      this.$emit("orderedBurger", {
        name: this.burger.name,
        amount: this.amountOrdered});
    }
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/*.burger {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  margin: 8px;
  text-align: center;
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.burger img {
  max-width: 30%;
  height: auto;
  border-radius: 4px;
}

.burger h3 {
  margin: 8px 0;
}

.Allergi {
  color: red;
  font-weight: bold;
}*/
</style>