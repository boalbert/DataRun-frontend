<template>
	<div class="flex bg-white rounded-sm shadow-md p-6 gap-2">
		<form @submit.prevent="searchByDate(searchParams)" class=" flex gap-2">
			<input
				type="date"
				name="start"
				class=" border-gray-300 p-1 text-xs"
				v-model.trim="searchParams.start"
			/>

			<input
				type="date"
				name="end"
				class="border-gray-300 p-1 text-xs"
				v-model.trim="searchParams.end"
			/>

			<button
				type="submit"
				class="bg-red-500 text-white text-xs 
                font-bold uppercase rounded hover:bg-red-400
                  p-2"
			>
				Search
			</button>
		</form>

		<button
			class="bg-blue-500 text-white text-xs 
                font-bold uppercase rounded hover:bg-red-400
                  p-2"
			@click="clearSearch"
		>
			Clear
		</button>
	</div>
</template>

<script>
export default {
	name: 'SearchBar',
	data() {
		return {
			searchParams: {
				start: '',
				end: '',
			},
		}
	},
	methods: {
		searchByDate(searchParams) {
			this.$emit('search-by-date', searchParams)
			console.log('Emitted: ')
			console.log(searchParams.start)
			console.log(searchParams.end)
		},
		clearSearch() {
			this.$emit('clear-search')
		},
	},
}
</script>

<style></style>
