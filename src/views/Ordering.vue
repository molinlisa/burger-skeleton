<template>

  <div id="ordering">

    <div id="OrderingShow" v-if="!finishView">
      <!-- Menyknappar högst upp i gränssnittet -->
      <div id="menyFlexBox">
        <MenuButtons v-for="cat in listMenuTitles"
        :title="cat.title"
        :ui-labels="uiLabels"
        v-on:selected="changeCategory"
        :category="cat.cat"
        :currentCategory="currentCategory">
      </MenuButtons>
    </div>

    <!-- Header "Ingredients" -->
    <h1 id="h1">{{ uiLabels.ingredients }}</h1>
    <!-- Checkboxes för matpreferenser -->
    <div id="foodPref">
      <label for="lactose">{{ uiLabels.milkFree }}</label>
      <input type="checkbox" id="lactose" v-bind:name="uiLabels.milkFree" :value="true" v-model="iNeedLactoseFree">
      <label for="vegan">{{ uiLabels.vegan }}</label>
      <input type="checkbox" id="vegan" v-bind:name="uiLabels.vegan" :value="true" v-model="iNeedVegan">
      <label for="gluten">{{ uiLabels.gluten }}</label>
      <input type="checkbox" id="gluten" v-bind:name="uiLabels.gluten" :value="true" v-model="iNeedGlutenFree">
    </div>
    <!-- Skriver ut ingredienser och dess "increment" knappar (läggs till sorder) -->
    <div id="orderContainer">
      <div id="ingredientBox">
        <Ingredient
        ref="ingredient"
        v-for="item in currentIngredients"
        v-on:increment="addToBurger(item)"
        :item="item"
        :lang="lang"
        :ui-labels="uiLabels"
        :key="item.ingredient_id">
      </Ingredient>


    </div>

    <!-- Header "My burger" -->
    <div id="gridContainer" >
      <h1 id="h1">{{ uiLabels.order }}</h1>
      <div id="count">
        <h4>Antal</h4>
        <div v-for="(item,key2 ) in groupIngredients(chosenIngredients)" class="countingCol">
          <button v-on:click="removeItem(item.ing)" class="minusButton">-</Button>
            <h5 class="countNumb">{{item.count}}</h5>
            <button v-on:click="addToBurger(item.ing)" class="plusButton">+</button>
          </div>
        </div>

        <div id="foodList">
          <h4>Food-name</h4>
          <p v-for="(item,key2 ) in groupIngredients(chosenIngredients)">{{item.ing["ingredient_"+lang]}}</p>
        </div>

        <div id="price">
          <h4>Price </h4>
          <h4 v-for="(item,key2 ) in groupIngredients(chosenIngredients)">{{item.ing["selling_price"]*item.count}}</h4>
        </div>

        <kryss ref="kryss">
          <h4>Remove item</h4>
          <div v-for="(item,key2) in groupIngredients(chosenIngredients)" v-on:click="removeAll(item)"><img src="https://fyrhjuling.se/wp-content/uploads/2018/12/dirtbike-cross.jpg" width="20"></div>
        </kryss>

      </div>
      {{ chosenIngredients.map(item => item["ingredient_"+lang]).join(', ') }}, {{ price }} {{uiLabels.sek}}
      <button v-on:click="addToOrder()">Add burger</button>
    </div>
  </div>
  <div v-else>
    <!-- Header "Orders in queue" -->
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

    <div class="footer">
      <h1>{{ uiLabels.order }}</h1>
      <div v-for="(burger, key) in currentOrder.burgers" :key="key">
        {{"burger "}}  {{key+1}}:
        <h4 v-for="(item, key2) in groupIngredients(burger.ingredients)" :key="key2">
          {{item.count}} {{ item.ing['ingredient_' + lang] }}
        </h4>
        {{burger.price}}
      </div>
      <hr>
      <button v-on:click="addToOrder()">Add burger</button>
      <button v-on:click="placeOrder()">{{ uiLabels.placeOrder }}</button>
      <button v-on:click="addAnotherBurger()">Lägg till ny burgare</button>
    </div>

    <button v-on:click="switchLang()">{{ uiLabels.language }}</button>
  </div>
</div>
</div>

</template>
<script>
//import the components that are used in the template, the name that you
//use for importing will be used in the template above and also below in
//components
import Ingredient from '@/components/Ingredient.vue'
import OrderItem from '@/components/OrderItem.vue'
import MenuButtons from '@/components/MenuButtons.vue'
//import methods and data that are shared between ordering and kitchen views
//import methods and data that are shared between ordering and kitchen views
import sharedVueStuff from '@/mixins/sharedVueStuff.js'
import utilityMethods from '@/mixins/utilityMethods.js'
/* instead of defining a Vue instance, export default allows the only
necessary Vue instance (found in main.js) to import your data and methods */
export default {
  name: 'Ordering',
  components: {
    Ingredient,
    OrderItem,
    MenuButtons
  },
  mixins: [sharedVueStuff,utilityMethods], // include stuff that is used in both
  // the ordering system and the kitchen
  data: function() { //Not that data is a function!
    return {
      chosenIngredients: [],
      price: 0,
      orderNumber: "",
      listMenuTitles: [{title:"bread",cat:4},{title:"patty",cat:1},{title:"addOn",cat:2},{title:"sauce",cat:3},{title:"extras",cat:5},{title:"beverage",cat:6}],
      currentCategory: 4,
      iNeedLactoseFree: false,
      iNeedGlutenFree: false,
      iNeedVegan: false,
      currentOrder: {
        burgers: []
      },
      finishView: false
    }
  },

  computed: {
    currentIngredients: function () {
      let ing = [];
      for(let a = 0; a < this.ingredients.length; a += 1) {
        if (this.ingredients[a].category === this.currentCategory) {

          preferences: {
            if(this.iNeedLactoseFree == true && this.ingredients[a].milk_free == 0) {
              break preferences;
            }
            if(this.iNeedGlutenFree == true && this.ingredients[a].gluten_free == 0){
              break preferences;
            }
            if(this.iNeedVegan == true && this.ingredients[a].vegan == 0){
              break preferences;
            }
            ing.push(this.ingredients[a]);
          }
        }
      }
      return ing;
    },
  },
  created: function () {
    this.$store.state.socket.on('orderNumber', function (data) {
      this.orderNumber = data;
    }.bind(this));
  },
  methods: {
    changeCategory: function(cat) {
      this.currentCategory = cat;
    },
    addToBurger: function (item) {
      this.chosenIngredients.push(item);
      this.price += +item.selling_price;
    },
    addToOrder: function () {
      // Add the burger to an order array;
      if (this.chosenIngredients.length > 0) {
        this.currentOrder.burgers.push({
          ingredients: this.chosenIngredients.splice(0),
          price: this.price
        });
        //set all counters to 0. Notice the use of $refs
        this.finishView = true;
        for (let i = 0; i < this.$refs.ingredient.length; i += 1) {
          this.$refs.ingredient[i].resetCounter();
        }
        this.chosenIngredients = [];
        this.price = 0;
      }
    },
    placeOrder: function () {
      // make use of socket.io's magic to send the stuff to the kitchen via the server (app.js)
      if (this.currentOrder.burgers.length > 0){
        this.currentOrder.time = Date.now();
        this.$store.state.socket.emit('order', this.currentOrder);
        this.currentOrder.burgers = [];
        this.category = 1;
      }
    },
    countNumberOfIngredients: function (id) {
      let counter = 0;
      for (let j = 0; j < this.chosenIngredients.length; j += 1) {
        if(this.chosenIngredients[j].ingredient_id === id){
          counter+= 1
        }
      }
      return counter;
    },
    removeItem: function(item){
      let removeIndex = 0;
      for (let i = 0; i < this.chosenIngredients.length; i += 1 ) {
        if (this.chosenIngredients[i] === item) {
          removeIndex = i;
          break;
        }
      }
      this.chosenIngredients.splice(removeIndex, 1);
      this.price -= +item.selling_price;
    },
    addAnotherBurger: function(){
      this.finishView = false;

    },
    removeAll: function(item){
      console.log(item.count)
      while(item.count > 0){
        removeItem(item.ing);
      }




    }
  }
}
</script>
<style scoped>
/* scoped in the style tag means that these rules will only apply to elements, classes and ids in this template and no other templates. */
#ordering {
  height: 100%;
  width: 100%;
  background-color: white;
  /*background: url(https://previews.123rf.com/images/solarus/solarus1510/solarus151000011/45890246-tablecloth-background-red-seamless-pattern-vector-illustration-of-traditional-gingham-dining-cloth-w.jpg); */
  opacity:0.9;
  color: white;
}
#foodPref{
  font-size: 1.5em;
  text-indent: 2em;
}
#h1{
  text-indent: 2em;
}
#menyFlexBox{
  display: flex;
  flex-direction: row;
  justify-content: center;
}
#orderContainer{
  display:grid;
}
#ingredientBox{
  grid-column: 1;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  height: 45vh;
  margin: 5vh;
  width: 120vh;
  overflow: overlay;
  word-break: break-word;
}
#gridContainer{
  display: grid;
  grid-column: 2;
  grid-template-areas: 'header header header header'
  'main main main main';
  grid-template-rows: min-content 1fr;
}
#gridContainer h1{
  grid-area: header;
}
#foodList{
  grid-area: main;
  grid-column: 3;
  grid-row: 2;
}
#price{
  grid-area:main;
  grid-column: 2;
  grid-row: 2;
}
#count{
  grid-area:main;
  grid-column: 1;
  grid-row: 2;
}
.ingredient {
  border: 1px solid #f5f5f28a;
  padding: 0.8em;
  width: 23vh;
  height: 19vh;
  background-color: green;
  border-radius: 25px;
  margin: 10px;
  text-align: center;
}
.kryss{
  grid-area:main;
  grid-column: 4;
}
.countingCol{
  display: grid;
}
.minusButton{
  grid-column: 1;
  width: 0.5vh;
  border-radius: 50px;
  background-color: red;
}
.countNumb{
  grid-column: 2;
  margin: 1px;
}
.plusButton{
  grid-column: 3;
  width: 0.5vh;
  border-radius: 50px;
  background-color: lightblue;
}
.footer{
  display: grid;
}
</style>
