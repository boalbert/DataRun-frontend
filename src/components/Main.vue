<template>
	<div class="flex pt-14 gap-8 p-3 bg-gray-100 justify-between">
		<div class="flex flex-col w-96 gap-4">
			<ProfileCard>
				<template v-slot:logo>
					<i class="fas fa-fill fa-3x fa-user-circle"></i>
				</template>
				<template v-slot:username>
					Total
				</template>
				<template v-slot:totalDistance> {{ totalDistance }} </template>
				<template v-slot:countRuns> {{ countRuns }} </template>
				<template v-slot:totalTime> {{ totalTime }} </template>
			</ProfileCard>

			<ProfileCard>
				<template v-slot:username>
					This Week
				</template>
				<template v-slot:totalDistance> {{ dataThisWeek.distance }} </template>
				<template v-slot:countRuns> {{ dataThisWeek.runs }} </template>
				<template v-slot:totalTime> {{ dataThisWeek.duration }} </template>
			</ProfileCard>

			<EmptyCard>
				<template v-slot:bar>
					<Bar :data="testData" :options="options" />
				</template>
			</EmptyCard>
		</div>

		<div class="flex flex-wrap flex-col gap-4 w-96">
			<SearchBar
				@search-by-date="searchByDate"
				@clear-search="fillActivitiesList"
			/>
			<Card
				v-for="(activity, index) in activitiesByDate"
				v-bind:key="activity.id"
			>
				<template v-slot:date>
					ID#{{ activity.id }} | {{ activity.date }} | {{ activity.shoes }}
				</template>
				<template v-slot:title>
					{{ activity.name }}
				</template>

				<template v-slot:stats>
					{{ activity.distance }} km | {{ activity.avgHr }} hr |
					{{ activity.duration }} min |
					{{ (activity.duration / activity.distance).toFixed(2) }}/km
				</template>

				<template v-slot:delete>
					<button
						@click="deleteActivity(index, activity.id)"
						class="font-semibold uppercase text-gray-300"
					>
						<i class="fas fa-times"></i>
					</button>
				</template>
			</Card>
		</div>
		<div class="w-96">
			<PostRun @submit-activity="postNewActivity" />
		</div>
	</div>
</template>

<script>
import Card from '@/components/Card.vue'
import ProfileCard from '@/components/ProfileCard.vue'
import PostRun from '@/components/PostRun.vue'
import EmptyCard from '@/components/EmptyCard.vue'
import SearchBar from '@/components/SearchBar.vue'
import { Bar } from 'vue-chart-3'
import { computed, ref } from 'vue'

export default {
	name: 'Main',
	components: {
		Card,
		ProfileCard,
		PostRun,
		EmptyCard,
		Bar,
		SearchBar,
	},

	setup() {
		const dataValues = ref([5.5, 2, 10.11, 2, 2, 14, 50])

		let options = {
			responsive: false,
			maintainAspectRatio: false,
		}

		const testData = computed(() => ({
			labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
			datasets: [
				{
					barPercentage: 1.0,

					label: 'Week',
					data: dataValues.value,
					backgroundColor: [
						'#77CEFF',
						'#0079AF',
						'#123E6B',
						'#97B0C4',
						'#A5C8ED',
					],
				},
			],
		}))
		return { testData, options }
	},
	data() {
		return {
			allActivites: [],
			activitiesThisWeek: [],
			dataLoaded: false,
			CurrentWeekStartDate: '',
			CurrentWeekEndDate: '',
		}
	},

	methods: {
		getAllActivities() {
			return new Promise((resolve, reject) => {
				fetch(process.env.VUE_APP_ROOT_URL + 'activities/')
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

		async searchByDate(searchParams) {
			this.allActivities = await this.getByDate(
				searchParams.start,
				searchParams.end
			)
		},

		getByDate(start, end) {
			return new Promise((resolve, reject) => {
				fetch(
					process.env.VUE_APP_ROOT_URL +
						`activities/search?start=${start}&end=${end}`
				)
					.then((response) => response.json())
					.then((activities) => {
						resolve((this.allActivites = activities))
						this.dataLoaded = true
					})
					.catch((err) => {
						console.log(err)
						console.log(reject)
					})
			})
		},
		async fillActivitiesList() {
			this.allActivites = await this.getAllActivities()
		},

		async fillActivitiesThisWeekList() {
			this.activitiesThisWeek = await this.getByDate(
				this.CurrentWeekStartDate,
				this.CurrentWeekEndDate
			)
		},

		async postNewActivity(activityDetails) {
			const rawResponse = await fetch(
				process.env.VUE_APP_ROOT_URL + 'activities/',
				{
					method: 'POST',
					headers: {
						Accept: 'application/json',
						'Content-Type': 'application/json',
					},
					body: JSON.stringify(activityDetails),
				}
			)

			const content = await rawResponse.json()

			this.allActivites.push(content)
		},
		deleteActivity(index, id) {
			fetch(process.env.VUE_APP_ROOT_URL + `activities/${id}`, {
				method: 'DELETE',
			}).then(this.allActivites.splice(index, 1))

			console.log('Trying to delete / splice /activities/ at: ')
			console.log(`id: ${id} - index: ${index}`)
		},
		currentWeek() {
			let currentdate = new Date()
			let oneJan = new Date(currentdate.getFullYear(), 0, 1)
			let numberOfDays = Math.floor(
				(currentdate - oneJan) / (24 * 60 * 60 * 1000)
			)
			let result = Math.ceil((currentdate.getDay() + 1 + numberOfDays) / 7)

			return result
		},
		calculateCurrentWeek() {
			let today = new Date()
			let nextWeek = new Date(
				today.getUTCFullYear(),
				today.getUTCMonth(),
				today.getUTCDate() + 7
			)
			let start = today.toLocaleDateString('sv-SE')
			let end = nextWeek.toLocaleDateString('sv-SE')
			return [start, end]
		},
	},
	computed: {
		activitiesByDate() {
			const sortedactivities = this.allActivites
				.slice()
				.sort((a, b) => new Date(b.date) - new Date(a.date))

			return sortedactivities
		},

		totalDistance() {
			const totalDistance = this.allActivites.reduce(function(acc, curr) {
				return acc + curr.distance
			}, 0)

			return totalDistance.toFixed(0)
		},
		countRuns() {
			return this.allActivites.length
		},
		totalTime() {
			let totalTime = this.allActivites.reduce(function(acc, curr) {
				return acc + curr.duration
			}, 0)

			totalTime = totalTime / 60
			return totalTime.toFixed(2)
		},

		runningPace(time, distance) {
			return time / distance
		},

		// dataThisWeek() {

		// 	let distanceThisWeek = this.activitiesThisWeek.reduce(function(acc, curr) {
		// 		return acc + curr.distance
		// 	}, 0)

		// 	return distanceThisWeek
		// },

		dataThisWeek() {
			let statsThisWeek = this.activitiesThisWeek

			let statsObject = {
				distance: '',
				duration: '',
				runs: '',
			}

			// let timeThisWeek = this.activitiesThisWeek
			statsObject.distance = statsThisWeek.reduce(function(acc, curr) {
				return acc + curr.distance
			}, 0)

			statsObject.duration = statsThisWeek
				.reduce(function(acc, curr) {
					return acc + curr.duration
				}, 0)
				.toFixed(2)

			statsObject.runs = statsThisWeek.length

			return statsObject
		},
	},
	mounted() {
		this.CurrentWeekStartDate = this.calculateCurrentWeek()[0]
		this.CurrentWeekEndDate = this.calculateCurrentWeek()[1]
		this.fillActivitiesList()
		this.fillActivitiesThisWeekList()
	},
}
</script>
