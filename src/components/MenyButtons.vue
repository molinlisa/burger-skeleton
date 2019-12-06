<template>
<div>
  <button class="MenyButtons" v-on:click="select">{{ uiLabels[title] }} </button>
</div>
</template>

<script>

export default {
  name: 'MenyButtons',
  props: {
    uiLabels: Object,
    lang: String,
    title: String
  },
data: function() { //Not that data is a function!
  return {
  }
},
 methods: {
select() {
    this.$emit("selected", this.title)
  }
}
}
</script>

<style scoped>

.is-grey{
  background: grey;
}

.is-green{
  background: green;
}

.MenyButtons{
  width:100px;
  height:100px;
}

.MenyButtons:focus{
  background-color: red;
}

</style>
