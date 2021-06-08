<template>
	<div class="bg-white rounded-sm shadow-lg p-4 justify-center flex">
		<div class="mx-auto">
			<slot name="bar">Bar Chart</slot>
		</div>
	</div>
</template>

<script>
export default {
	name: 'EmptyCard',
}
</script>
