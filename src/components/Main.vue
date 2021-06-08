<template>
	<div
		class="flex flex-col gap-12 p-8 bg-gray-100 lg:flex-row xl:flex-row 2xl:flex-row justify-center"
	>
		<div class="flex gap-4 flex-col">
			<ProfileCard>
				<template v-slot:username>
					Total
				</template>
				<template v-slot:totalDistance> {{ totalDistance }} </template>
				<template v-slot:countRuns> {{ countRuns }} </template>
				<template v-slot:totalTime> {{ totalTime }} </template>
			</ProfileCard>

			<ThisWeekCard />

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
import ThisWeekCard from '@/components/ThisWeekCard.vue'
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
		ThisWeekCard,
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
			dataLoaded: false,
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
	},
	mounted() {
		this.fillActivitiesList()
	},
}
</script>
