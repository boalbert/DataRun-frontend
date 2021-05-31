<template>
	<header class="flex drop-shadow-2xl p-4 bg-white bg-opacity-95 pl-24">
		<div>
			<h1
				class="font-sans tracking-widest text-4xl text-red-400 text-opacity-70 font-bold ml-4 "
			>
				<i class="fas fa-running"></i> datarun
			</h1>
		</div>
		<div>
			<nav
				class="flex flex-row text-base font-semibold tracking-wider cursor-pointer text-gray-300 space-x-2 ml-16"
			>
				<a class="bg-gray-900 p-2 pl-5 pr-5 rounded-lg text-gray-200" href="#"
					>Dashboard</a
				>
				<a class="p-2 pl-5 pr-5 rounded-lg hover:bg-gray-700" href="#"
					>Settings</a
				>
				<a class="p-2 pl-5 pr-5 rounded-lg hover:bg-gray-700" href="#">About</a>
			</nav>
		</div>
	</header>
</template>

<script>
export default {
	name: 'Header',
}
</script>

<style></style>
