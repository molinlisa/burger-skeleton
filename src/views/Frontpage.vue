<template>
  <div onclick="window.location.href='#/';" style="cursor:pointer; width:100%; height:100%;">
    <div id="textpage"> <h2>Goda burgare </h2>
      <h3> - Klicka för att ta dig till hamburgarhimlen </h3>

    </div>


  </div>
</template>

<script>
export default {
  name: 'frontpage',
}
</script>

<style scoped>
#header {
  border: 1px solid;
  padding: 1em;
  color: orange;
  font-size: 5 rem;
  font-family: Algerian;
}
#headerimage {

  opacity: 0.5;
  width: 100%;
  height: auto;
  margin: auto;

}
h3{
  color: white;
  font-family: cursive;
}
#textpage {
color: white;
font-family: Algerian;
position: absolute;
text-align: center;
font-size: 25px;
height: 100%;
width: 100%;
padding: 5px;
  background: url('https://nordicdine.com/wp-content/uploads/2018/09/hamburgare_allt_du_behover_veta.jpg');
  background-size: 100vw;
}
</style>
