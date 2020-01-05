<template>
  <!-- Note in this component that it is using another component -->
  <div>
    <OrderItem
    :ui-labels="uiLabels"
    :lang="lang"
    :order-id="orderId"
    :order="order"
    :time="time">
  </OrderItem>
  <button id="doneButton" v-on:click="orderDone">
    {{uiLabels.ready}}
  </button>
</div>
</template>
<script>
import OrderItem from '@/components/OrderItem.vue'

export default {
  name: 'OrderItemToPrepare',
  components: { OrderItem },
  props: {
    uiLabels: Object,
    order: Object,
    orderId: String,
    lang: String,
    time: String
  },
  methods: {
    orderDone: function () {
      // sending 'done' message to parent component or view so that it
      // can catch it with v-on:done in the component declaration
      this.$emit('done');
    },
    cancelOrder: function () {
      // not implemented
    }
  }
}
</script>
<style scoped>

#doneButton {
  background-color: green;
  font-size: 40px;
  font-weight: bold;
  width: 180px;
  height: 100px;
}

</style>
