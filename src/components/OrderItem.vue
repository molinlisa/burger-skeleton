<template>
	<div>
		<h3>
			{{uiLabels.headlineOrder}} {{orderId}} <br>
 			{{time}}
		</h3>
			<ul v-for="(burger,key) in order.burgers" :key="key">
				<h3>Burger {{key+1}} </h3>
				<li v-for="(ingredient, key2) in burger.ingredients">
					{{ingredient["ingredient_"+ lang]}} </li>
			</ul>
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
		time: String,
    lang: String
  }
}
</script>
<style scoped>


</style>
