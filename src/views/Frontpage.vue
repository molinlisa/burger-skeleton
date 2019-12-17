<template>

  <div onclick="window.location.href='#/ordering';" style="cursor:pointer; width:100%; height:100%;">
    <div id="textpage"> <h2>Crafty Burgers </h2>
      <h3> Klicka för att ta dig till hamburgarhimlen </h3>
    </div>
  </div>
</template>

<script>

export default {
  name: 'frontpage',
}
</script>

<style scoped>

h3{
  color: white;
  font-family: cursive;
}
#textpage {
color: white;
font-family: Algerian;
position: absolute;
text-align: center;
line-height: 90px;
font-size: 25px;
height: 100%;
width: 100%;
padding: 5px;
  background: url('https://nordicdine.com/wp-content/uploads/2018/09/hamburgare_allt_du_behover_veta.jpg');
  background-size: 100vw;
}
</style>
