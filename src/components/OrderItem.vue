<template>
	<div>


		<h3>
			{{time}} {{uiLabels.headlineOrder}} {{orderId}}
		</h3>

			<ul v-for="(item,index) in order.ingredients" :key="index">
				<li> {{item["number"]}} {{item["ingredient_" + lang]}} </li>
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
  }
}
</script>
<style scoped>

</style>
