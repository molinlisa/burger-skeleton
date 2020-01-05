<template>
  <div class="wrapper">
        <div onclick="window.location.href='#/ordering';" style="cursor:pointer; width:100%; height:100%;">
            <div id="textpage">
                <h2> Crafty Burgers </h2>
                <h3> {{ uiLabels.welcomeText }} </h3>
            </div>
        </div>
        <button @click="switchLang()">{{ uiLabels.language }}</button>
    </div>
</template>

<script>
import sharedVueStuff from '@/mixins/sharedVueStuff.js';
import Ordering from '@/views/Ordering.vue';

export default {
  name: 'frontpage',
  mixins: [sharedVueStuff, Ordering],
}
</script>

<style scoped>

.wrapper {
   position: realtive;
}
button {
  background-color: green;
  color: white;
  width: 80px;
  height: 40px;
  font-weight: bold;
  position: absolute;
  bottom: 20px;
  left: 20px;
}
#textpage {
  color: black;
  /* font-family: Algerian; */
  font-weight: bold;
  text-align: center;
  line-height: 90px;
  font-size: 25px;
  background: url(https://cdn5.vectorstock.com/i/1000x1000/26/39/burger-on-hand-fast-food-vector-22992639.jpg);

  /* Set rules to fill background */
  min-height: 100%;
  min-width: 1024px;

  /* Set up proportionate scaling */
  width: 100%;
  height: auto;

  /* Set up positioning */
  position: fixed;
  top: 0;
  left: 0;
}
</style>
