<template>
	<div class="bg-gray-200 h-screen pt-3">
		<Header class="p-2 w-5/6 mx-auto" />
		<Home class="p-2 w-5/6 mx-auto" />
	</div>
</template>

<script>
import Home from '@/components/Home.vue'
import Header from '@/components/Header.vue'
export default {
	name: 'App',
	components: {
		Header,
		Home,
	},
}
</script>
