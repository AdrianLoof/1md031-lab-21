<template>


  <div>
    <figure>
      <h3 v-bind:key=burger.name> {{ burger.name }}</h3>
      <img v-bind:src="burger.img" style="width: 200px;">
      <ul>
        <li v-bind:key=burger.kCal><span class="ingrediens">{{ burger.kCal }}</span> Calories</li>
        <li v-if="burger.lactose">Contains <span class="ingrediens">Lactose</span></li>
        <li v-else>Lactose <span class="ingrediens">Free</span></li>
        <li v-if="burger.gluten"> Contains <span class="ingrediens">Gluten</span></li>
        <li v-else> Gluten <span class="ingrediens">Free</span></li>
      </ul>
      {{ amountOrdered }}
      <button v-on:click="addBurger()" className="button" style="margin-left: 20px  " type="add">
        Add Burger
      </button>
      <button v-on:click="removeBurger()" className="button" style="margin-left: 20px  " type="subtract">
        Remove Burger
      </button>
    </figure>
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
  methods: {
    addBurger: function () {
      this.amountOrdered += 1
      this.$emit('orderedBurger', {
            name: this.burger.name,
            amount: this.amountOrdered
          }
      );
    },
    removeBurger: function () {
      this.amountOrdered -= 1
      this.$emit('orderedBurger', {
            name: this.burger.name,
            amount: this.amountOrdered
          }
      );
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#testid {
  text-transform: uppercase;
  background-color: black;
  color: white;
  border: dotted;

}

.wrapper {
  display: grid;
  grid-gap: 10px;
  padding: 10px;
  background-color: #726d6d;
  color: #111010;
}


.a {
  grid-column: 1;
  border: groove #ff5500;
}
</style>
