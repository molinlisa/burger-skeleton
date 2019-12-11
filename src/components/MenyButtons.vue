<template>
<div>
  <button :class="['MenyButtons', {'is-green' : currentCategory === category}]" v-on:click="select">{{ uiLabels[title] }} </button>
</div>
</template>

<script>

export default {
  name: 'MenyButtons',
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

.MenyButtons{
  width:100px;
  height:100px;
  border-radius: 12px;
  padding: 14px 28px;
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
