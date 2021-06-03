<template>
	<div class="flex flex-col bg-white rounded-sm shadow-lg p-8 h-auto">
		<h2 class="text-1xl font-semibold text-center">
			<slot name="username">Enter Run</slot>
		</h2>

		<button @click="open = !open">
			<i v-if="!open" class="fas fa-caret-down text-2xl text-gray-600"></i>
			<i v-if="open" class="fas fa-sort-up text-2xl text-gray-600"></i>
		</button>

		<transition name="slide-fade">
			<form
				class=" text-xs"
				@submit.prevent="submitActivity(activityDetails)"
				v-if="open"
			>
				<!-- Name -->
				<label class="block mt-4 text-gray-700 font-semibold" for="name"
					>Name
					<input
						class="block w-full mt-1 border-gray-400 text-gray-600 placeholder-gray-300 bg-white rounded"
						type="text"
						name="name"
						placeholder="Skatås 5km..."
						id=""
						v-model.trim="activityDetails.name"
					/>
				</label>

				<!-- Date -->
				<label class="block mt-4 text-gray-700 font-semibold" for="date"
					>Date
					<input
						class="block w-full mt-1 border-gray-400 text-gray-600 placeholder-gray-300 bg-white rounded"
						type="date"
						name="date"
						id=""
						v-model.trim="activityDetails.date"
					/>
				</label>

				<!-- Distance -->
				<label class="block mt-4 text-gray-700 font-semibold" for="distance"
					>Distance
					<input
						class="block w-full mt-1 border-gray-400 text-gray-600 placeholder-gray-300 bg-white rounded"
						type="text"
						name="distance"
						placeholder="In kilometers..."
						id=""
						v-model.trim="activityDetails.distance"
					/>
				</label>

				<!-- Time -->
				<label for="duration" class="block mt-4 text-gray-700 font-semibold"
					>Duration

					<input
						class="block w-full mt-1 border-gray-400 text-gray-600 placeholder-gray-300 bg-white rounded"
						type="text"
						name="duration"
						id=""
						placeholder="53:27"
						v-model.trim="activityDetails.duration"
					/>
				</label>

				<!-- HR -->
				<label for="avgHr" class="block mt-4 text-gray-700 font-semibold"
					>HR</label
				>
				<input
					class="block w-full mt-1 border-gray-400 text-gray-600 placeholder-gray-300 bg-white rounded"
					type="text"
					name="avgHr"
					id=""
					placeholder="168"
					v-model.trim="activityDetails.avgHr"
				/>

				<!-- Elevation -->
				<label for="elevation" class="block mt-4 text-gray-700 font-semibold"
					>Elevation</label
				>
				<input
					class="block w-full mt-1 border-gray-400 text-gray-600 placeholder-gray-300 bg-white rounded"
					type="text"
					name="elevation"
					id=""
					placeholder="120"
					v-model.trim="activityDetails.elevation"
				/>

				<!-- Shoes -->
				<label class="block text-left mt-4">
					<span class="text-gray-700 font-semibold">Shoes</span>
					<select
						class="form-select block w-full mt-1 rounded border-gray-400"
						v-model.trim="activityDetails.shoes"
					>
						<option>Nike Pegasus</option>
						<option>Altra Lonepeak</option>
					</select>
				</label>

				<button
					class="bg-red-500 text-white p-4 font-bold uppercase rounded mt-4 w-1/2 mx-auto hover:bg-red-400"
					type="submit"
				>
					Submit
				</button>
			</form>
		</transition>
	</div>
</template>

<script>
export default {
	name: 'PostRun',
	data() {
		return {
			activityDetails: {
				name: '',
				duration: '',
				distance: '',
				date: '',
				elevation: '',
				avgHr: '',
				shoes: '',
			},
			open: false,
		}
	},
	methods: {
		submitActivity(activityDetails) {
			this.$emit('submit-activity', activityDetails)
			this.activityDetails.name = ''
			this.activityDetails.duration = ''
			this.activityDetails.distance = ''
			this.activityDetails.date = ''
			this.activityDetails.elevation = ''
			this.activityDetails.avgHr = ''
			this.activityDetails.shoes = ''
		},
	},
}
</script>

<style scoped>
.slide-fade-enter-active {
	transition: all 0.3s ease;
}
.slide-fade-leave-active {
	transition: all 0.15s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter,
.slide-fade-leave-to {
	transform: translateY(-25px);
	opacity: 0;
}
</style>
