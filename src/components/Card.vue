<template>
	<div class="flex flex-col bg-gray-300 w-80 rounded-lg shadow-xl mt-10">
		<img
			class="h-40 w-auto rounded-t-lg object-cover filter"
			src="https://source.unsplash.com/random"
		/>
		<h2 class="text-2xl font-semibold pt-5 text-center">
			<slot name="title">Placeholder</slot>
		</h2>
		<p class="text-left p-5">
			<slot name="body">
				Lorem ipsum dolor sit, amet consectetur adipisicing elit. Deserunt
				possimus provident laborum explicabo dignissimos vero pariatur quia
				porro deleniti modi ullam voluptatem fugiat consequuntur, rerum
				voluptate cum? Corrupti, suscipit tempore?</slot
			>
		</p>
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
