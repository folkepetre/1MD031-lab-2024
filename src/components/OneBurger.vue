<template>
  <div class="burger">
    <h3> {{ burger.name }}</h3>
    <img v-bind:src= "burger.img" 
      alt="burger" width="360" height="280" />
    <ul>
      <li> {{ burger.kcal }} kcal</li>
      <li v-if="burger.gluten">Contains <span class="ingredient">gluten</span></li>
      <li v-if="burger.lactose">Contains <span class="ingredient">lactose</span></li>
    </ul>
    <div class="orderButtons">
      <button id="removeButton"v-on:click="amountOrdered > 0 && amountOrdered--">-</button>
      {{ amountOrdered }}
       <button id="addButton" v-on:click="addBurger">+</button>
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
    padding: 40px;
    display: flex;
    flex-direction: column;
    
}
.ingredient {
    font-weight: bold;
}

.orderButtons{
  margin-top: auto;
  display: flex;
  gap: 8px;
  align-items: center;
  padding: 0px 115px;
}

</style>