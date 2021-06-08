<template>
	<div class="bg-white rounded-sm shadow-md p-6 justify-center flex">
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
