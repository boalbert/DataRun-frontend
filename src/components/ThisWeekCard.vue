<template>
	<div class="flex flex-col bg-white rounded-sm shadow-md p-6">
		<h2 class="text-2xl font-semibold text-center text-gray-400 mb-4">
			<slot name="username">Week {{ currentWeekNumber }}</slot>
		</h2>

		<div>
			<div
				class="flex gap-2 space text-xs border-b p-1 text-gray-800 font-medium"
				v-for="activity in activitiesThisWeek"
				v-bind:key="activity.id"
			>
				<div class=" w-12">{{ formatDate(activity.date) }}</div>
				<div class="w-24">{{ activity.name }}</div>
				<div class="w-16">{{ activity.distance.toFixed(1) }}km</div>
				<div class=" w-16">{{ (activity.duration / 60).toFixed(2) }}h</div>
				<div class=" w-16">
					{{ (activity.duration / activity.distance).toFixed(2) }}min/km
				</div>
			</div>
		</div>

		<div class="text-center grid grid-cols-3 mt-8 font-semibold">
			<div class="text-gray-400">
				Runs
			</div>
			<div class="text-gray-400">Distance</div>
			<div class="text-gray-400">Time</div>

			<div class="text-1xl font-bold">
				<slot name="countRuns">{{ countRuns }}</slot>
			</div>
			<div class="text-1xl font-bold">
				<slot name="totalDistance"> {{ totalDistance }} </slot>km
			</div>

			<div class="text-1xl font-bold">
				<slot name="totalTime">{{ totalTime }}</slot
				>h
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'ThisWeekStats',
	data() {
		return {
			dataLoaded: false,
			activitiesThisWeek: [],
			currentWeekNumber: Number,
		}
	},
	// activities/search/week?week=20&year=2021
	methods: {
		getWeekStats(week, year) {
			return new Promise((resolve, reject) => {
				fetch(
					`${process.env.VUE_APP_ROOT_URL}activities/search/week?week=${week}&year=${year}`
				)
					.then((response) => response.json())
					.then((activities) => {
						resolve(activities)
						this.dataLoaded = true
					})
					.catch((err) => {
						console.log(err)
						console.log(reject)
					})
			})
		},
		async fillActivitiesThisWeek() {
			this.activitiesThisWeek = await this.getWeekStats(
				this.currentWeekNumber,
				2021
			)
		},
		calculateCurrentWeek() {
			let currentdate = new Date()
			let oneJan = new Date(currentdate.getFullYear(), 0, 1)
			let numberOfDays = Math.floor(
				(currentdate - oneJan) / (24 * 60 * 60 * 1000)
			)
			let currentWeek = Math.ceil((currentdate.getDay() + 1 + numberOfDays) / 7)

			return currentWeek
		},
		formatDate(dateString) {
			const date = new Date(dateString)
			// Then specify how you want your dates to be formatted
			let formattedDate = Intl.DateTimeFormat('default', {
				dateStyle: 'long',
			}).format(date)

			return formattedDate.substring(0, formattedDate.length - 5)
		},
	},

	computed: {
		totalDistance() {
			const totalDistance = this.activitiesThisWeek.reduce(function(acc, curr) {
				return acc + curr.distance
			}, 0)

			return totalDistance.toFixed(0)
		},
		countRuns() {
			return this.activitiesThisWeek.length
		},

		totalTime() {
			let totalTime = this.activitiesThisWeek.reduce(function(acc, curr) {
				return acc + curr.duration
			}, 0)

			totalTime = totalTime / 60
			return totalTime.toFixed(2)
		},
	},
	mounted() {
		this.currentWeekNumber = this.calculateCurrentWeek()
		this.fillActivitiesThisWeek()
	},
}
</script>
