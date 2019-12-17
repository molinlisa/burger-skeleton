<template>
	<div>
		<h3>
			{{time}}	{{uiLabels.headlineOrder}} {{orderId}}
		</h3>
<<<<<<< HEAD
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

=======
			<ul v-for="(item,index) in order.ingredients" :key="index">
				<li> {{item["number"]}} {{item["ingredient_" + lang]}} </li>
  		</ul>
>>>>>>> 9ecbb1e18cb837e405a6e4f9a0da0e550c45c81d
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
