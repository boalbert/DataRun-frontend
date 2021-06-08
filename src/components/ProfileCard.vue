<template>
	<div class="flex flex-col bg-white rounded-sm shadow-md p-6">
		<h2 class="text-2xl font-semibold text-center text-gray-400 mb-4">
			<slot name="username">Title</slot>
		</h2>

		<div class="text-center grid grid-cols-3 font-semibold">
			<div class="text-gray-400">
				Runs
			</div>
			<div class="text-gray-400">Distance</div>
			<div class="text-gray-400">Time</div>

			<div class="text-1xl font-bold">
				<slot name="countRuns">DefaultRuns</slot>
			</div>
			<div class="text-1xl font-bold">
				<slot name="totalDistance">DefaultDistance</slot>km
			</div>

			<div class="text-1xl font-bold">
				<slot name="totalTime">DefaultTime</slot>h
			</div>

			<slot name="chart"></slot>
		</div>
	</div>
</template>

<script>
export default {
	name: 'ProfileCard',
}
</script>

<style></style>
