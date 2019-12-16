<template>
<div>
  <button :class="['MenuButtons', {'is-green' : currentCategory === category}]" v-on:click="select">{{ uiLabels[title] }} </button>
</div>
</template>

<script>

export default {
  name: 'MenuButtons',
  props: {
    uiLabels: Object,
    lang: String,
    title: String,
    category: Number,
    currentCategory: Number
  },
data: function() { //Note that data is a function!
  return {
  }
},
 methods: {
select() {
    this.$emit("selected", this.category)
  }
}
}
</script>

<style scoped>

.MenuButtons{
  width:25vh;
  height:20vh;
  border-radius: 12px;
  padding: 14px 28px;
  text-align: center;
  font-size: 17pt;
  font-weight: bold;
}

.is-grey{
  background: grey;
  border-color: black;
  color: black;
}

.is-green{
  background: green;
  font-weight: bold;
  color: white;
  border-color: white;
}

</style>
