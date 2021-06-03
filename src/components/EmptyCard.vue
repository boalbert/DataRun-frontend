<template>
	<div class="bg-white rounded-sm shadow-lg p-4 justify-center flex">
		<div class="mx-auto">
			<slot>Default text</slot>
		</div>
	</div>
</template>

<script>
export default {
	name: 'EmptyCard',
}
</script>
