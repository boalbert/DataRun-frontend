<template>
	<div class="flex flex-col bg-white rounded-sm shadow-lg p-4">
		<div class="flex justify-between">
			<h2 class="text-1xl font-bold">
				<i class="fas fa-running mr-2"></i
				><slot name="title">Placeholder Title</slot>
			</h2>
			<slot name="delete">X</slot>
		</div>

		<h3 class=" text-xs font-normal text-gray-500">
			<slot name="date">Placeholder Date</slot>
		</h3>

		<div class="pt-5 font-semibold pb-5">
			<p class="text-left font-medium">
				<slot name="stats"> Placeholder body text</slot>
			</p>
		</div>

		<img
			class="h-32 w-auto object-cover filter"
			src="https://source.unsplash.com/random"
		/>
	</div>
</template>

<script>
export default {
	name: 'Card',
	props: {
		activites: Array,
		
	},
}
</script>
