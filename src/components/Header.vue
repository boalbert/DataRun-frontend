<template>
	<div class="flex flex-col">
		<header
			class="flex rounded-t-lg drop-shadow-2xl p-4 bg-gray-900 bg-opacity-95"
		>
			<div>
				<h1
					class="font-sans tracking-widest text-4xl text-blue-200 text-opacity-70 font-semibold ml-4 "
				>
					<i class="fas fa-running"></i> datarun
				</h1>
			</div>
			<div></div>
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
					<a class="p-2 pl-5 pr-5 rounded-lg hover:bg-gray-700" href="#"
						>About</a
					>
				</nav>
			</div>
		</header>
		<div class="bg-gray-100 border-b-4 border-gray-300">
			<h2 class="text-3xl font-bold p-6">Dashboard</h2>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Header',
}
</script>

<style></style>
