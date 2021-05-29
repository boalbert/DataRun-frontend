<template>
	<div class="flex flex-wrap gap-8 justify-between">
		<Card>
			<template v-slot:title>
				Title text
			</template>
			<template v-slot:body>
				Some body text sent from another component.
			</template>
		</Card>
		<Card>
			Another title
		</Card>
		<Card>
			Third title
		</Card>
	</div>
</template>

<script>
import Card from '@/components/Card.vue'

export default {
	name: 'Home',
	components: {
		Card,
	},
}
</script>
