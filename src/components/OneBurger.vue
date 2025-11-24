<template>
  <div class="burger">
    <h3> {{ burger.name }}</h3>
    <img v-bind:src= "burger.img" 
      alt="burger" width="500" height="380" />
    <ul>
      <li> {{ burger.kcal }} kcal</li>
      <li v-if="burger.gluten">Contains <span class="ingredient">gluten</span></li>
      <li v-if="burger.gluten">Contains <span class="ingredient">lactose</span></li>
    </ul>
    <div>
      <button class="addButton" v-on:click="addBurger">+</button>
      <button class="removeButton"v-on:click="amountOrdered > 0 && amountOrdered--">-</button>
      <p>Amount ordered: {{ amountOrdered }}</p>
  </div>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
  return {
    amountOrdered: 0,
  }
},
  methods: {
    addBurger: function () {
      this.amountOrdered += 1;
      this.$emit('orderedBurger', 
      { name:   this.burger.name, 
        amount: this.amountOrdered 
      }
  );
},
}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.burger {
    margin: 10px;

}
.ingredient {
    font-weight: bold;
}

</style>