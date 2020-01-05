<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import Ordering from "./views/Ordering.vue"

export default {
  name: 'app',
  components: {
    Ordering
  }
}
</script>

<style>
body {
  background: url(https://previews.123rf.com/images/lhfgraphics/lhfgraphics1212/lhfgraphics121200023/16929379-doodle-stile-hamburger-e-hot-dog-barbecue-cortile-sfondo-senza-soluzione-di-continuit%C3%A0.jpg);
  /* https://cdn5.vectorstock.com/i/1000x1000/95/14/delicious-hamburger-fast-food-background-icon-vector-14199514.jpg);
  /* url(https://i.pinimg.com/564x/7a/74/2e/7a742edc9c4820e8871c835b013d93ee.jpg); /* url(https://i.pinimg.com/564x/85/25/d2/8525d271aa0e5756acf70ed427ddb35d.jpg); */
  /* font: url(https://fonts.googleapis.com/css?family=B612&display=swap); */
}
</style>
