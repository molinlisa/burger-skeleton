<template>

  <div class="ingredient" v-on:click="incrementCounter" >
    <h3>{{item["ingredient_"+ lang]}}</h3>
    <p>{{item.selling_price}}{{uiLabels.sek}}</p>
  </div>
</template>
<script>
export default {
  name: 'Ingredient',
  props: {
    item: Object,
    lang: String,
    uiLabels: Object,
  },
  data: function () {
    return {
      counter: 0
    };
  },
  methods: {
    incrementCounter: function () {
      this.counter += 1;
      // sending 'increment' message to parent component or view so that it
      // can catch it with v-on:increment in the component declaration
      this.$emit('increment');
    },
    removeIng: function(){


    },
    resetCounter: function () {
      this.counter = 0;
    }
  }
}
</script>

<style scoped>

h3::first-letter {
  text-transform: capitalize;
}

</style>
