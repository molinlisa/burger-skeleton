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
  background: url(https://i.pinimg.com/564x/7a/74/2e/7a742edc9c4820e8871c835b013d93ee.jpg); /* url(https://i.pinimg.com/564x/85/25/d2/8525d271aa0e5756acf70ed427ddb35d.jpg); */
  /* font: url(https://fonts.googleapis.com/css?family=B612&display=swap); */
}
</style>
