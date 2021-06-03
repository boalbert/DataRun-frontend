<template>
	<Header />
	<Home />
</template>

<script>
import Home from '@/views/Home.vue'
import Header from '@/components/Header.vue'

export default {
	components: {
		Header,
		Home,
	},
	mounted() {
		console.log(process.env.VUE_APP_ROOT_URL)
	},
}
</script>
