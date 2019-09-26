<template>
	<Index/>
</template>

<script>
	import Index from './components/index'

	export default {
		name: 'App',
		components: {
			Index
		}
	}
</script>

<style>
	html {
		width: 100%;
		height: 100%;
		font-size: 14px;
	}
	body {
		max-width: 100%;
		width: 100%;
		font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
		-webkit-font-smoothing: antialiased;
		position: relative;
		padding: 0;
		margin: 0;
	}
	html, body {
		-webkit-overflow-scrolling: touch !important;
		height: 100% !important;
	}
</style>
