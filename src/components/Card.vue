<template>
	<div class="flex flex-col bg-white rounded-sm shadow-lg p-8">
		<h2 class="text-2xl font-bold">
			<i class="fas fa-running mr-2"></i
			><slot name="title">Placeholder Title</slot>
		</h2>
		<h3 class="text text-sm font-normal text-gray-500">
			<slot name="date">Placeholder Date</slot>
		</h3>
		<div class="pl-5 pb-0 pt-5 font-semibold block"></div>
		<p class="text-left pb-5 text-2xl font-medium">
			<slot name="stats"> Placeholder body text</slot>
		</p>
		<img
			class="h-60 w-auto object-cover filter"
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
