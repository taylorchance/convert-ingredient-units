<script>
import Counter from './Counter.svelte'
import Ingredient from './Ingredient.svelte'
// import welcome from '$lib/images/svelte-welcome.webp'
// import welcome_fallback from '$lib/images/svelte-welcome.png'
import { parseIngredient } from '@jlucaspains/sharp-recipe-parser'

let ingredients = ''
let parsedIngredients = []

const handleSubmit = () => {
	ingredients.split('\n').forEach(ingredient => {
		const parsed = parseIngredient(ingredient, 'en')
		parsedIngredients = [...parsedIngredients, parsed]
	})
}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section class="section">
	<div class="container">
		<h1 class="title">
			Convert ingredients
		</h1>

		{#if !parsedIngredients.length}
			<div class="field">
				<h3 class="subtitle">Enter list of ingredients</h3>
				<textarea class="textarea" bind:value={ingredients}></textarea>
				<button class="button mt-3" on:click={handleSubmit}>Convert!</button>
			</div>

		{:else}
		
			<div class="columns">
				<div class="column">
					<h3 class="subtitle">Original</h3>
					{#each parsedIngredients as obj}
						<Ingredient {obj} />
					{/each}
				</div>
				<div class="column">
					<h3 class="subtitle">Metric</h3>
					{#each parsedIngredients as obj}
						<Ingredient {obj} isMetric={true} />
					{/each}
				</div>
			</div>
		{/if}

		<!--
		9 cups cooked chicken approx (see note 1 to cook from scratch)
		2 slices bread or 3, if small, crusts removed
		1/3 cup milk 80ml
		1 onion
		2 cloves garlic
		3 tbsp walnuts
		3 tbsp grated parmesan or a little more, to taste
		2 tbsp vegetable oil
		2 tsp aji amarillo chili paste or more to taste
		1/4 tsp turmeric
		1/2 cup chicken stock (or reserved poaching liquid)

		480 g cooked chicken approx (see note 1 to cook from scratch)
		2 slices bread or 3, if small, crusts removed
		80 ml milk 80ml
		1 onion
		2 cloves garlic
		30 g walnuts
		10 g grated parmesan or a little more, to taste
		30 ml vegetable oil
		10 ml aji amarillo chili paste or more to taste
		1.25 ml turmeric
		120 ml chicken stock (or reserved poaching liquid)
		-->

		<!-- <Counter /> -->
	</div>
</section>
