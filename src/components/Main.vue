<template>
	<div class="flex pt-14 gap-x-40 pl-10 bg-gray-100 ">
		<div class="flex flex-col w-96 gap-8 max-w-4xl">
			<ProfileCard>
				<template v-slot:username>
					Albert Andersson
				</template>
				<template v-slot:totalDistance> {{ totalDistance }} </template>
				<template v-slot:countRuns> {{ countRuns }} </template>
				<template v-slot:totalTime> {{ totalTime }} </template>
			</ProfileCard>

			<PostRun @submit-activity="postNewActivity" />
		</div>

		<div class="flex flex-wrap flex-col gap-8 w-4/12">
			<Card
				v-for="(activity, index) in activitiesByDate"
				v-bind:key="activity.id"
			>
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

				<template v-slot:delete>
					<button
						@click="deleteActivity(index, activity.id)"
						class="font-semibold uppercase text-gray-700"
					>
						<i class="fas fa-times"></i>
					</button>
				</template>
			</Card>
		</div>
	</div>
</template>

<script>
import Card from '@/components/Card.vue'
import ProfileCard from '@/components/ProfileCard.vue'
import PostRun from '@/components/PostRun.vue'
// import BarChart from '@/components/BarChart.vue'

export default {
	name: 'Main',
	components: {
		Card,
		ProfileCard,
		PostRun,
		// BarChart,
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
				fetch('http://localhost:5050/activities')
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
		async fillActivitiesList() {
			this.allActivites = await this.getAllActivities()
		},
		async postNewActivity(activityDetails) {
			const rawResponse = await fetch('http://localhost:5050/activities', {
				method: 'POST',
				headers: {
					Accept: 'application/json',
					'Content-Type': 'application/json',
				},
				body: JSON.stringify(activityDetails),
			})

			const content = await rawResponse.json()

			this.allActivites.push(content)
		},
		deleteActivity(index, id) {
			fetch(`http://localhost:5050/activities/${id}`, {
				method: 'DELETE',
			}).then(this.allActivites.splice(index, 1))
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
