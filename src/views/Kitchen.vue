<template>
  <div id="orders">
    <h1 id>{{ uiLabels.ordersInQueue }} </h1>
    <div class = "wrapper">
      <OrderItemToPrepare class = "box ordered"
      v-for="(order, key) in orders"
      v-if="order.status !== 'done'"
      v-on:done="markDone(key)"
      :order-id="key"
      :order="order"
      :ui-labels="uiLabels"
      :lang="lang"
      :time="new Date(order.time).toLocaleTimeString('it-IT')"
      :key="key">
    </OrderItemToPrepare>
  </div>

  <h1 id>{{ uiLabels.ordersFinished }}</h1>

  <div class = "wrapper">
    <OrderItem class = "box finished"
    v-for="(order, key) in orders"
    v-if="order.status === 'done'"
    :order-id="key"
    :order="order"
    :lang="lang"
    :ui-labels="uiLabels"
    :time="new Date(order.time).toLocaleTimeString('it-IT')"
    :key="key">
  </OrderItem>
</div>
</div>
</template>
<script>
import OrderItem from '@/components/OrderItem.vue'
import OrderItemToPrepare from '@/components/OrderItemToPrepare.vue'

//import methods and data that are shared between ordering and kitchen views
import sharedVueStuff from '@/mixins/sharedVueStuff.js'

export default {
  name: 'Ordering',
  components: {
    OrderItem,
    OrderItemToPrepare
  },
  mixins: [sharedVueStuff], // include stuff that is used in both
  //the ordering system and the kitchen
  data: function(){
    return {
      chosenIngredients: [],
      price: 0
    }
  },
  methods: {
    markDone: function (orderid) {
      this.$store.state.socket.emit("orderDone", orderid);
    }
  }
}
</script>

<style scoped>
h1 {
  text-transform: uppercase;
  font-size: 1.4em;
  color: bisque;
  text-indent: 2em;
}
#orders {
  font-size:24pt;
}
#headlines {
  color: white;
}
#flexList{
  display: flex;
  flex-direction: column;
}
h1 {
  text-transform: uppercase;
  font-size: 1.4em;
  color: orange;
  font-weight: bold;
  text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;

}
.wrapper {
  display: grid;
  grid-gap: 50px;
  grid-template-columns: repeat(auto-fill, 10em);
  grid-template-rows: auto;
  justify-content: center;
  background-color: bisque;
  color: black;
  border: 10px solid black;
  border-radius: 25px;
}
.box {
  border-radius: 25px;
  padding: 20px;
  border: 5px solid black;
}
.ordered {
  background-color: burlywood;
}
.finished {
  background-color: goldenrod;
}
</style>
