<template>
  <div id="ordering">
    <button class="Clear"  v-on:click="clearOrderAndRedirect()"> {{uiLabels.cancel}}</button>
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
    <!-- Skriver ut ingredienser och dess "increment" knappar (läggs till sorder) här yalla -->
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
        <h4>{{ uiLabels.nr }}</h4>
        <div v-for="(item,key2 ) in groupIngredients(chosenIngredients)" class="countingCol">
          <img v-on:click="removeItem(item.ing)" class="minusButton" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQozMtJvJPf8PPZz4N5OIy9fdiEGIJohGHkp8UrPpSk0U8I4BSAsQ&s">
          <p class="countNumb">{{item.count}}</p>
          <img v-on:click="addToBurger(item.ing)" class="plusButton" src="https://cdn4.iconfinder.com/data/icons/ui-3d-01-of-3/100/UI_2-512.png">
        </div>
      </div>

      <div id="foodList">
        <h4>{{ uiLabels.chosenIngredients }}</h4>
        <p v-for="(item,key2 ) in groupIngredients(chosenIngredients)">{{item.ing["ingredient_"+lang]}}</p>
      </div>

      <div id="price">
        <h4>{{ uiLabels.price }}</h4>
        <p v-for="(item,key2 ) in groupIngredients(chosenIngredients)">{{item.ing["selling_price"]*item.count}}</p>
      </div>

      <div id="kryss">
        <h4>{{ uiLabels.removeItem }}</h4>
        <div v-for="(item,key2) in groupIngredients(chosenIngredients)" v-on:click="removeAll(item.ing)"><img src="https://cdn2.iconfinder.com/data/icons/media-and-navigation-buttons-round/512/Button_12-512.png" width="30"></div>
      </div>

      {{uiLabels.totalPrice}} {{this.ingredientsTotPrice}} {{uiLabels.sek}}
    </div>
    <!-- {{ chosenIngredients.map(item => item["ingredient_"+lang]).join(', ') }}, {{ price }} {{uiLabels.sek}} -->
    <button id="addToOrderButton" v-on:click="addToOrder()">{{ uiLabels.goToOrder }}</button>
  </div>
  <button v-on:click="switchLang()">{{ uiLabels.language }}</button>
</div>

  <!-- Go to order view -->
  <div v-else>
    <div class="footer">
      <h1>{{ uiLabels.order }}</h1>
      <div id="burgerInOrder" v-for="(burger, key) in currentOrder.burgers" :key="key">
        {{'Meny'}}  {{key+1}}
        <img id="editButton" v-on:click="editButton(burger)" src="http://www.edubizsoft.com/images/icons/Image.png" width="20">
        <img v-on:click="removeButton(burger)" src="https://image.flaticon.com/icons/png/512/458/458594.png" width="20">
        <p v-for="(item, key2) in groupIngredients(burger.ingredients)" :key="key2">
          {{item.count}} {{ item.ing['ingredient_' + lang] }}
        </p>
        {{uiLabels.price}} {{burger.price}} {{uiLabels.sek}}
      </div>
      <p>
      {{uiLabels.totalPrice}} {{this.currentOrder.totPrice}} {{uiLabels.sek}}
      </p>
      </div>
      <hr>
      <button id="ordinaryButton" v-on:click="addAnotherBurger()">{{ uiLabels.addNewBurger }}</button>

  <div>
    <transition name="modal">
      <div v-if="isOpen">
        <div class="overlay">
          <div class="modal" data-backdrop="static" data-keyboard="false">
            <h1>{{uiLabels.thankOrder}}</h1>
            <p>{{uiLabels.byeByeText}} {{orders[0]}}</p>
            <button class="Clear"  v-on:click="clearOrderAndRedirect()"> {{uiLabels.finish}}</button>
          </div>
        </div>
      </div>
    </transition>
    <button id="ordinaryButton" v-on:click="placeOrder()">{{ uiLabels.placeOrder }}
    </button>
  </div>
    <button id="languageButton" v-on:click="switchLang()">{{ uiLabels.language }}</button>
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
      isOpen: false,
      ingredientsTotPrice: 0,
      currentOrder: {
        burgers: [],
        totPrice: 0
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
      this.totPriceIngredientsFunc();
    },
    totPriceFunc: function() {
      this.currentOrder.totPrice = 0;
      for (let j=0; j<this.currentOrder.burgers.length; j+=1) {
        this.currentOrder.totPrice += this.currentOrder.burgers[j].price;
      }
    },
    totPriceIngredientsFunc: function() {
      this.ingredientsTotPrice = 0;
      var ingredients = this.groupIngredients(this.chosenIngredients);
      for (let j=0; j<ingredients.length; j+=1) {
        this.ingredientsTotPrice += ingredients[j].ing['selling_price']*ingredients[j].count
      }
    },
    addToOrder: function () {
      // Lägg till bugare om vi har någon, skicak till beställning om vi får
      if (this.chosenIngredients.length > 0) {
        this.currentOrder.burgers.push({
          ingredients: this.chosenIngredients.splice(0),
          price: this.price
        });
        //set all counters to 0. Notice the use of $refs
        for (let i = 0; i < this.$refs.ingredient.length; i += 1) {
          this.$refs.ingredient[i].resetCounter();
        }
        this.chosenIngredients = [];
        this.price = 0;
      }
      console.log(this.currentOrder.totPrice);
      this.totPriceFunc();
      this.totPriceFunc();
      this.totPriceIngredientsFunc()
      if(this.currentOrder.burgers.length > 0) {
        this.finishView = true;
      }
    },
    placeOrder: function () {
      // make use of socket.io's magic to send the stuff to the kitchen via the server (app.js)
      if (this.currentOrder.burgers.length > 0 || this.currentOrder.length > 0){
        this.currentOrder.time = Date.now();
        this.$store.state.socket.emit('order', this.currentOrder);
        this.currentOrder.burgers = [];
        this.category = 1;
        this.isOpen = true;
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
      let lengthOfArray = this.chosenIngredients.length;
      let removeIndex = 0;

      for(let i = 0; i < lengthOfArray;){
        if(this.chosenIngredients[i] === item) {
          removeIndex = i;
          this.chosenIngredients.splice(removeIndex, 1);
          this.price -= +item.selling_price;
        }
        else {i += 1}
      }
    },

    editButton: function(burger) {
      // create copy of ingredients array for chosenIngredients
      this.chosenIngredients = burger.ingredients.slice(0);
      // update price. Could as well use for loop instead of reducer
      this.price = this.chosenIngredients.reduce(function (acc, cur) {
        return acc + cur.selling_price;
      }, 0)

      let removeIndex = 0;
      for (let i = 0; i < this.currentOrder.length; i += 1 ) { //vill ta bort valda ingredienser/tillagd burgare från currentOrder (s a ej dubbelt)
        if (this.currentOrder.burgers[i] === burger) {
          removeIndex = i;
          break;
        }
      }
      this.currentOrder.burgers.splice(removeIndex, 1);
      this.finishView = false;
    },

    removeButton: function(burger){
      let removeIndex = 0;
      for (let i = 0; i < this.currentOrder.length; i += 1 ) { //vill ta bort valda ingredienser/tillagd burgare från currentOrder (s a ej dubbelt)
        if (this.currentOrder.burgers[i] === burger) {
          removeIndex = i;
          break;
        }
      }
      this.currentOrder.burgers.splice(removeIndex, 1);
      this.totPriceFunc();
    },

    clearOrderAndRedirect: function() {
      this.chosenIngredients = [];
      this.price = 0;
      this.currentOrder.burgers = [];
      this.category = 1;
      window.location.href = '#/';
    }
  }
}
</script>
<style scoped>
/* scoped in the style tag means that these rules will only apply to elements, classes and ids in this template and no other templates. */
#ordering {
  height: 100%;
  width: 100%;
  opacity:0.9;
  color: white;
}
#foodPref{
  font-size: 2em;
  text-indent: 2em;
  color: orange;
  font-weight: bold;
  text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;
}
input[type="checkbox"] {
  height: 20px;
  width: 20px;
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
  width: 100vh;
  overflow: overlay;
  word-break: break-word;
}
#gridContainer{
  display: grid;
  grid-column: 2;
  grid-template-areas: 'header header header header'
  'main main main main';
  grid-template-rows: min-content 1fr;
  background-color: black;
  padding: 1em;
  margin-left: -15px;
  margin-top: -6px;
  border-radius: 25px;
  grid-column-gap: 25px;
  text-align: center;
  overflow: hidden;
  width:500px;
}
#gridContainer h1{
  grid-area: header;
}
#foodList{
  grid-area: main;
  grid-column: 3;
  grid-row: 2;
}
#foodList p {
  text-align: left;
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
#ordinaryButton{
  width: 200px;
  height: 40px;
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
.ingredient:active {
  background-color: orange;
  color: green;
}
.kryss {
  grid-area:main;
  grid-column: 4;
}
#addToOrderButton {
  grid-area: main;
  grid-column: 2;
  grid-row: 3;
  border-radius: 25px;
  margin: 4px;
  align: right;
  width: 120px;
  height: 70px;
}
.countingCol{
  display: grid;
  padding-bottom: 14px;
}
.Clear{
  width: 90px;
  height: 60px;
  background-color: red;
  font-weight: bold;
  font-size-adjust: auto;
  border-radius: 12px;
}
.minusButton{
  grid-column: 1;
  border-radius: 50px;
  width: 2.6vh;
}
.plusButton{
  grid-column: 3;
  width: 3vh;
  border-radius: 50px;
}
.countNumb{
  grid-column: 2;
  margin: 1px;
}
.footer{
  display: grid;
  color: orange;
  background-color: black;
  text-align: left;
  width: 30%;
  margin-left: 35%;
  text-indent: 10px;
}
#burgerInOrder {
  margin-bottom: 1em;
}
#burgerInOrder p {
  text-indent: 2em;
}
.buttons {
  width:25vh;
  height:20vh;
  border-radius: 12px;
}
#editButton{
  margin-right: 5px;
}
.modal {
  width: 500px;
  margin: 0px auto;
  padding: 20px;
  background-color: black;
  border-radius: 2px;
  box-shadow: 0 2px 8px 3px;
  transition: all 0.2s ease-in;
  font-family: Helvetica, Arial, sans-serif;
  color: pink;
}
.fadeIn-enter {
  opacity: 0;
}

.fadeIn-leave-active {
  opacity: 0;
  transition: all 0.2s step-end;
}

.fadeIn-enter .modal,
.fadeIn-leave-active.modal {
  transform: scale(1.1);
}
button {
  padding: 7px;
  margin-top: 10px;
  background-color: green;
  color: white;
  font-size: 1.1rem;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background: #00000094;
  z-index: 999;
  transition: opacity 0.2s ease;
}

</style>
