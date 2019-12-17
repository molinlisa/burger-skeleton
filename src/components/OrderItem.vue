<template>
	<div>


		<h3>
			{{time}}	{{uiLabels.headlineOrder}} {{orderId}}
		</h3>
<<<<<<< HEAD
			{{order.burgers[1]}}
			<div v-for="(burger,key) in order.burgers" :key="key">
				{{ burger.ingredients.map(item=>item["ingredient_"+ lang]).join(", ") }}
  		</div>
=======

			<ul v-for="(item,index) in order.ingredients" :key="index">
				<li> {{item["number"]}} {{item["ingredient_" + lang]}} </li>
  		</ul>


>>>>>>> d1a42c36616871472c2a772637658a39bacc4c1a

		<!-- {{orderId}} {{order.type}} {{uiLabels.ingredients}}: {{ order.ingredients.map(item=>item["ingredient_"+ lang]).join(", ") }} -->
	</div>
</template>
<script>
export default {
  name: 'OrderItem',
  props: {
    uiLabels: Object,
    order: Object,
    orderId: String,
    lang: String,
		time: String
  }
}
</script>
<style scoped>

</style>
