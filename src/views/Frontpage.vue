<template>
  <div class="wrapper">
        <div onclick="window.location.href='#/ordering';" style="cursor:pointer; width:100%; height:100%;">
            <div id="textpage"> <h2> Crafty Burgers </h2>
                <h3> {{ uiLabels.welcomeText }} </h3>
            </div>
        </div>
        <button class="knappshuno" @click="switchLang()">{{ uiLabels.language }}</button>
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
h3{
  color: white;
  font-family: Algerian;
}
.wrapper {
   position: realtive;
}
.knappshuno {
   position: absolute;
   bottom: 20px;
   left: 20px
}
#textpage {
  color: white;
  font-family: Algerian;
  position: absolute;
  text-align: center;
  line-height: 90px;
  font-size: 25px;
  min-height: 100%;
  min-width: 100%;
  background: url('https://images.fineartamerica.com/images-medium-large-5/pattern-with-fast-food-hand-drawn-colored-icons-doodle-illustrations-of-burger-pizza-slice-ice-cream-vector-background-with-food-illustrations-for-bistro-fast-food-chains-restaurants-snack-bar-annaelesina.jpg');

}
</style>
