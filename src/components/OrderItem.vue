<template>
	<div>
		<h3>
			{{uiLabels.headlineOrder}} {{orderId}} <br>
 			{{time}}
		</h3>
<<<<<<< HEAD
			<div v-for="(burger,key) in order.burgers" :key="key">
			{{"Burger number "}}{{key+1}}:	{{ burger.ingredients.map(item=>item["ingredient_"+ lang]).join(", ") }}
  		</div>

=======
			<ul v-for="(burger,key) in order.burgers" :key="key">
				<h3>Burger {{key+1}} </h3>
				<li v-for="(ingredient, key2) in burger.ingredients">
					{{ingredient["ingredient_"+ lang]}} </li>
			</ul>
>>>>>>> 748156f2cfbeecf14f4bc52e790645c5ea0ea361
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
