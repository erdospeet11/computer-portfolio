<script lang="ts">
	import Desktop from '$lib/Desktop.svelte';
</script>

<svelte:head>
	<title>Windows Desktop Portfolio</title>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
	<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400;1,700&display=swap" rel="stylesheet">
</svelte:head>

<Desktop />

<style>
	:global(html, body) {
		margin: 0;
		padding: 0;
		height: 100%;
		overflow: hidden;
		font-family: 'Space Mono', 'Consolas', 'Courier New', monospace;
	}
</style>
