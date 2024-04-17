<script lang="ts">
export let obj
export let isMetric
let { ingredient, quantity, minQuantity, maxQuantity, unit } = obj

import { getDensity } from '$lib/utils'

if (isMetric) {
	const poundsArr = ['lb', 'lbs', 'pound', 'pounds']
	const cupsArr = ['cup', 'cups']

	if (poundsArr.includes(unit)) {
		unit = 'g'
		quantity = quantity * 454
		minQuantity = minQuantity * 454
		maxQuantity = maxQuantity * 454
	}
	
	if (cupsArr.includes(unit)) {
		const density = getDensity(ingredient)
		console.log('density', density)
		quantity = quantity * density.density
		minQuantity = minQuantity * density.density
		maxQuantity = maxQuantity * density.density
		unit = density.unit	
	}

	if (quantity > 1000 && unit === 'g') {
		quantity = quantity / 1000
		unit = 'kg'
	}
}
</script>

<div>
	{#if minQuantity < maxQuantity}
		<span class="has-text-weight-bold">
			{Math.round(minQuantity * 100) / 100} to {Math.round(maxQuantity * 100) / 100}
		</span>
	{:else if quantity}
		<span class="has-text-weight-bold">
			{Math.round(quantity * 100) / 100}
		</span>
	{/if}

	{#if unit}
		<span class="has-text-weight-bold">
			{unit}
		</span>
	{/if}

	{ingredient}
</div>

<style scoped>
div {
	border-bottom: 1px solid;
	padding: .5rem 0;
}
</style>