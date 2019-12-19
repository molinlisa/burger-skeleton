<template>
	<div>



		<h3>
			{{time}} {{uiLabels.headlineOrder}} {{orderId}}
					<h3 v-if data.lang = "en"> </h3>
			order: {{orderId}}
		</h3>
			<div v-for="(burger,key) in order.burgers" :key="key">
			{{"Burger number "}}{{key+1}}:	{{ burger.ingredients.map(item=>item["ingredient_"+ lang]).join(", ") }}
  		</div>

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
