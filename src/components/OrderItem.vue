<template>
	<div>


		<h3 v-if data.lang = "en">
			order: {{orderId}}
		</h3>

			<ul v-for="(item,index) in order.ingredients" :key="index">
				<li> {{item["ingredient_" + lang]}} </li>
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
    lang: String
  }
}
</script>
<style scoped>

<<<<<<< HEAD

=======
>>>>>>> 6ef59fd1962c300dfcc1fee7fcef8f0b07892b4b
</style>
