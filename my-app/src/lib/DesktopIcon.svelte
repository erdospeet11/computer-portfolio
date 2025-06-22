<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	export let id: string;
	export let name: string;
	export let icon: string;
	export let x: number;
	export let y: number;
	export let selected: boolean = false;

	const dispatch = createEventDispatcher();
	let dragStartX = 0;
	let dragStartY = 0;
	let isDragging = false;

	function handleMouseDown(event: MouseEvent) {
		isDragging = true;
		dragStartX = event.clientX - x;
		dragStartY = event.clientY - y;
		
		event.preventDefault();
		event.stopPropagation();
	}

	function handleMouseMove(event: MouseEvent) {
		if (isDragging) {
			x = event.clientX - dragStartX;
			y = event.clientY - dragStartY;
		}
	}

	function handleMouseUp() {
		isDragging = false;
	}

	function handleDoubleClick(event: MouseEvent) {
		event.preventDefault();
		event.stopPropagation();
		dispatch('doubleclick');
	}

	function handleClick(event: MouseEvent) {
		event.stopPropagation();
		dispatch('click');
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<svelte:window 
	on:mousemove={handleMouseMove} 
	on:mouseup={handleMouseUp}
/>

<div
	class="desktop-icon"
	class:selected
	style="left: {x}px; top: {y}px;"
	on:mousedown={handleMouseDown}
	on:dblclick={handleDoubleClick}
	on:click={handleClick}
>
	<div class="icon-image">{icon}</div>
	<div class="icon-label">{name}</div>
</div>

<style>
	.desktop-icon {
		position: absolute;
		display: flex;
		flex-direction: column;
		align-items: center;
		width: 80px;
		padding: 8px;
		cursor: pointer;
		user-select: none;
		border-radius: 4px;
		transition: background-color 0.1s;
	}

	.desktop-icon:hover {
		background-color: rgba(255, 255, 255, 0.1);
	}

	.desktop-icon.selected {
		background-color: rgba(0, 120, 212, 0.3);
		border: 1px solid rgba(0, 120, 212, 0.6);
	}

	.icon-image {
		font-size: 48px;
		margin-bottom: 4px;
		text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
	}

	.icon-label {
		color: white;
		font-size: 12px;
		text-align: center;
		text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.8);
		word-wrap: break-word;
		max-width: 80px;
		line-height: 1.2;
	}
</style> 