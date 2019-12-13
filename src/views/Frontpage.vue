<template>
  <div onclick="window.location.href='#/';" style="cursor:pointer; width:100%; height:100%;">
    <div id="textpage"> <h2>Goda burgare </h2>

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
#textpage {
color: orange;
font-family: Algerian;
position: absolute;
text-align: center;
font-size: 25px;
height: 80vh;
width: 100%;
padding: 5px;
  background: url('https://static.adweek.com/adweek.com-prod/wp-content/uploads/2018/10/Screen-Shot-2018-10-17-at-1.25.22-PM.png');
  background-size: 100vw;
}
</style>
