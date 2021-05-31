<template>
	<div class="flex pt-14 gap-x-40 pl-10 bg-gray-100">
		<ProfileCard>
			<template v-slot:username>
				Albert Andersson
			</template>
			<template v-slot:totalDistance> {{ totalDistance }} </template>
			<template v-slot:countRuns> {{ countRuns }} </template>
			<template v-slot:totalTime> {{ totalTime }} </template>
		</ProfileCard>
		<div class="flex flex-wrap flex-col gap-8 w-2/5">
			<Card v-for="activity in activitiesByDate" v-bind:key="activity.id">
				<template v-slot:date>
					{{ activity.date }}
				</template>
				<template v-slot:title>
					{{ activity.name }}
				</template>

				<template v-slot:stats>
					{{ activity.distance }} km | {{ activity.avgHr }} hr |
					{{ activity.duration }} min.
				</template>
			</Card>
		</div>
	</div>
</template>

<script>
import Card from '@/components/Card.vue'
import ProfileCard from '@/components/ProfileCard.vue'

export default {
	name: 'Main',
	components: {
		Card,
		ProfileCard,
	},
	data() {
		return {
			allActivites: [],
		}
	},
	methods: {
		getAllActivities() {
			return new Promise((resolve, reject) => {
				fetch('http://localhost:5050/activities')
					.then((response) => response.json())
					.then((activities) => {
						resolve(activities)
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
	},
	mounted() {
		this.fillActivitiesList()
	},
}
</script>
