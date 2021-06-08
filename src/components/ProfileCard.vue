<template>
	<div class="flex flex-col bg-white rounded-sm shadow-lg p-8">
		<h2 class="text-1xl font-bold text-center text-gray-400">
			<slot name="username">Firstname Lastname</slot>
		</h2>

		<div class="text-center grid grid-cols-3 mt-4 font-semibold">
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
