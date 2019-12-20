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
  background: url(https://i.pinimg.com/564x/85/25/d2/8525d271aa0e5756acf70ed427ddb35d.jpg);
}
</style>
