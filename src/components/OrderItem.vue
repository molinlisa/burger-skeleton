<template>
	<div>


<<<<<<< HEAD
		<h3>
			{{time}} {{uiLabels.headlineOrder}} {{orderId}}
=======
		<h3 v-if data.lang = "en">
			order: {{orderId}}
>>>>>>> 86d7cdcd6b6d852561c1f3ec03bee3e49c9f5a29
		</h3>
			<div v-for="(burger,key) in order.burgers" :key="key">
				{{ burger.ingredients.map(item=>item["ingredient_"+ lang]).join(", ") }}
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
<<<<<<< HEAD
    lang: String,
		time: String,
=======
    lang: String
>>>>>>> 86d7cdcd6b6d852561c1f3ec03bee3e49c9f5a29
  }
}
</script>
<style scoped>

</style>
