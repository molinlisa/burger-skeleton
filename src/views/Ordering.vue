<template>

  <div id="ordering">
    <!-- <img class="example-panel" src="@/assets/exampleImage.jpg">  bakgrundsbilden-->

    <!-- Menyknappar högst upp i gränssnittet -->
    <div id="menyFlexBox">
      <MenyButtons v-for="cat in listMenuTitles"
      :title="cat.title"
      :ui-labels="uiLabels"
      v-on:selected="changeCategory"
      :category="cat.cat">
      </MenyButtons>
    </div>

    <h1>{{ uiLabels.ingredients }}</h1>
    <h3> Only show: </h3>

    <!-- Checkboxes för matpreferenser -->
    <div id="foodPref">
      <label for="lactose">{{ uiLabels.milkFree }}</label>
      <input type="checkbox" id="lactose" v-bind:name="uiLabels.milkFree" :value="true" v-model="iNeedLactoseFree">
      {{lactose}}
      <label for="vegan">{{ uiLabels.vegan }}</label>
      <input type="checkbox" id="vegan" v-bind:name="uiLabels.vegan" :value="true" v-model="iNeedVegan">
      <label for="gluten">{{ uiLabels.gluten }}</label>
      <input type="checkbox" id="gluten" v-bind:name="uiLabels.gluten" :value="true" v-model="iNeedGlutenFree">
    </div>

    <!-- Skriver ut ingredienser och dess "increment" knappar (läggs till sorder) -->
    <Ingredient
      ref="ingredient"
      v-for="item in currentIngredients"
      v-on:increment="addToOrder(item)"
      :item="item"
      :lang="lang"
      :key="item.ingredient_id">
    </Ingredient>

    <h1>{{ uiLabels.order }}</h1>
    {{ chosenIngredients.map(item => item["ingredient_"+lang]).join(', ') }}, {{ price }} kr
    <button v-on:click="placeOrder()">{{ uiLabels.placeOrder }}</button>

    <h1>{{ uiLabels.ordersInQueue }}</h1>
    <div>
      <OrderItem
        v-for="(order, key) in orders"
        v-if="order.status !== 'done'"
        :order-id="key"
        :order="order"
        :ui-labels="uiLabels"
        :lang="lang"
        :key="key">
      </OrderItem>
    <button v-on:click="switchLang()">{{ uiLabels.language }}</button>
    </div>
  </div>
</template>
<script>
//import the components that are used in the template, the name that you
//use for importing will be used in the template above and also below in
//components
import Ingredient from '@/components/Ingredient.vue'
import OrderItem from '@/components/OrderItem.vue'
import MenyButtons from '@/components/MenyButtons.vue'
//import methods and data that are shared between ordering and kitchen views
import sharedVueStuff from '@/components/sharedVueStuff.js'
/* instead of defining a Vue instance, export default allows the only
necessary Vue instance (found in main.js) to import your data and methods */
export default {
  name: 'Ordering',
  components: {
    Ingredient,
    OrderItem,
    MenyButtons
  },
  mixins: [sharedVueStuff], // include stuff that is used in both
                            // the ordering system and the kitchen
  data: function() { //Not that data is a function!
    return {
      chosenIngredients: [],
      price: 0,
      orderNumber: "",
      listMenuTitles: [{title:"bread",cat:4},{title:"patty",cat:1},{title:"addOn",cat:2},{title:"sauce",cat:3},{title:"extras",cat:5},{title:"beverage",cat:6}],
      currentCategory: 1,
      iNeedLactoseFree: false,
      iNeedGlutenFree: false,
      iNeedVegan: false
    }
  },

  computed: {
    currentIngredients: function () {
      let ing = [];
      for(let a = 0; a < this.ingredients.length; a += 1) {
        if (this.ingredients[a].category === this.currentCategory) {
          preferences: {
          if(this.iNeedLactoseFree == true && Boolean(this.ingredients[a].milk_free) == false) {
          break preferences;
          }
          if(this.iNeedGlutenFree == true && Boolean(this.ingredients[a].gluten_free) == false){
          break preferences;
          }
          if(this.iNeedVegan == true && Boolean(this.ingredients[a].vegan) == false){
          break preferences;
          }
          ing.push(this.ingredients[a]);
            }
          }
        }
        return ing;
      },
      changeColor: function() { //här ändrar vi förhoppningsvis färgen på knappen vi väljer
        for (var i = 0; i < listMenuTitles.length; i++ ) {
          //givet att vi får reda på hur man selectar knappen baserat på i så ändrar vi färg
          // var knappshuno = document.getElementById(id);
          if (i == currentCategory) {
            //knappshuno.background = green;
          }
          else {
            //knappshuno.background = grey;
          }
        }
      }
    },
  created: function () {
    this.$store.state.socket.on('orderNumber', function (data) {
      this.orderNumber = data;
    }.bind(this));
  },
  methods: {
    changeCategory: function(cat) {
      this.currentCategory = cat;
      //this.cat
    },
    /*
    showMilkFree: function(item) {
      item.milk_free===1;
    },
    showVegan: function(item) {
      item.vegan===1;
    },
    showGlutenFree: function(item) {
      item.gluten_free===1;
    }, */
    addToOrder: function (item) {
      this.chosenIngredients.push(item);
      this.price += +item.selling_price;
    },
    placeOrder: function () {
      var i,
      //Wrap the order in an object
        order = {
          ingredients: this.chosenIngredients,
          price: this.price
        };
      // make use of socket.io's magic to send the stuff to the kitchen via the server (app.js)
      this.$store.state.socket.emit('order', {order: order});
      //set all counters to 0. Notice the use of $refs
      for (i = 0; i < this.$refs.ingredient.length; i += 1) {
        this.$refs.ingredient[i].resetCounter();
      }
      this.price = 0;
      this.chosenIngredients = [];
    }
  }
}
</script>
<style scoped>
/* scoped in the style tag means that these rules will only apply to elements, classes and ids in this template and no other templates. */
#ordering {
  margin:auto;
  width: 40em;
}
.example-panel {
  position: fixed;
  left:0;
  top:0;
  z-index: -2;
}
.ingredient {
  border: 1px solid #ccd;
  padding: 1em;
  background-image: url('~@/assets/exampleImage.jpg');
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: center;
  color: white;
}
#menyFlexBox{
  display: flex;
  flex-direction: row;
}
</style>
