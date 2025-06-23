<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import AboutWindow from './windows/AboutWindow.svelte';
	import ProjectsWindow from './windows/ProjectsWindow.svelte';
	import CardGameWindow from './windows/CardGameWindow.svelte';
	import ContactWindow from './windows/ContactWindow.svelte';
	import ResumeWindow from './windows/ResumeWindow.svelte';
	import WallpaperWindow from './windows/WallpaperWindow.svelte';
	import TerminalWindow from './windows/TerminalWindow.svelte';
	import CubeWindow from './windows/CubeWindow.svelte';
	import ImageWindow from './windows/ImageWindow.svelte';
	import TextWindow from './windows/TextWindow.svelte';
	import FileExplorerWindow from './windows/FileExplorerWindow.svelte';

	export let id: string;
	export let title: string;
	export let component: string;
	export let x: number;
	export let y: number;
	export let width: number;
	export let height: number;
	export let zIndex: number;
	export let props: any = {};

	const dispatch = createEventDispatcher();

	let isDragging = false;
	let isResizing = false;
	let dragStartX = 0;
	let dragStartY = 0;
	let resizeStartX = 0;
	let resizeStartY = 0;
	let resizeStartWidth = 0;
	let resizeStartHeight = 0;

	const components: { [key: string]: any } = {
		AboutWindow,
		ProjectsWindow,
		CardGameWindow,
		ContactWindow,
		ResumeWindow,
		WallpaperWindow,
		TerminalWindow,
		CubeWindow,
		ImageWindow,
		TextWindow,
		FileExplorerWindow
	};

	function handleTitleBarMouseDown(event: MouseEvent) {
		if (event.target === event.currentTarget || (event.target as HTMLElement).classList.contains('window-title')) {
			isDragging = true;
			dragStartX = event.clientX - x;
			dragStartY = event.clientY - y;
			dispatch('focus');
			event.preventDefault();
		}
	}

	function handleResizeMouseDown(event: MouseEvent) {
		isResizing = true;
		resizeStartX = event.clientX;
		resizeStartY = event.clientY;
		resizeStartWidth = width;
		resizeStartHeight = height;
		dispatch('focus');
		event.preventDefault();
		event.stopPropagation();
	}

	function handleMouseMove(event: MouseEvent) {
		if (isDragging) {
			x = Math.max(0, Math.min(window.innerWidth - width, event.clientX - dragStartX));
			y = Math.max(0, Math.min(window.innerHeight - height - 48, event.clientY - dragStartY));
		} else if (isResizing) {
			const deltaX = event.clientX - resizeStartX;
			const deltaY = event.clientY - resizeStartY;
			
			width = Math.max(300, Math.min(window.innerWidth - x, resizeStartWidth + deltaX));
			height = Math.max(200, Math.min(window.innerHeight - y - 48, resizeStartHeight + deltaY));
		}
	}

	function handleMouseUp() {
		isDragging = false;
		isResizing = false;
	}

	function handleClose() {
		dispatch('close');
	}

	function handleMinimize() {
		dispatch('minimize');
	}

	function handleWindowClick() {
		dispatch('focus');
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<svelte:window 
	on:mousemove={handleMouseMove} 
	on:mouseup={handleMouseUp}
/>

<div
	class="window"
	style="left: {x}px; top: {y}px; width: {width}px; height: {height}px; z-index: {zIndex};"
	on:click={handleWindowClick}
>
	<!-- Title Bar -->
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div class="title-bar" on:mousedown={handleTitleBarMouseDown}>
		<div class="window-title">{title}</div>
		<div class="window-controls">
			<button class="window-control minimize" on:click={handleMinimize}>−</button>
			<button class="window-control maximize">⬜</button>
			<button class="window-control close" on:click={handleClose}>×</button>
		</div>
	</div>

	<!-- Window Content -->
	<div class="window-content">
		{#if components[component]}
			<svelte:component this={components[component]} {...props} on:wallpaperChange />
		{:else}
			<div class="placeholder">
				<h2>{title}</h2>
				<p>Content for {component} will be displayed here.</p>
			</div>
		{/if}
	</div>

	<!-- Resize Handle -->
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div class="resize-handle" on:mousedown={handleResizeMouseDown}></div>
</div>

<style>
	.window {
		position: absolute;
		background: white;
		border: 1px solid #ddd;
		border-radius: 8px 8px 0 0;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
		overflow: hidden;
		min-width: 300px;
		min-height: 200px;
	}

	.title-bar {
		height: 32px;
		background: linear-gradient(to bottom, #f0f0f0, #e0e0e0);
		border-bottom: 1px solid #ccc;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0 8px;
		cursor: move;
		user-select: none;
	}

	.window-title {
		font-size: 13px;
		font-weight: 500;
		color: #333;
		flex: 1;
		pointer-events: none;
	}

	.window-controls {
		display: flex;
		gap: 2px;
	}

	.window-control {
		width: 24px;
		height: 20px;
		border: 1px solid #bbb;
		background: linear-gradient(to bottom, #fff, #f0f0f0);
		cursor: pointer;
		font-size: 12px;
		display: flex;
		align-items: center;
		justify-content: center;
		border-radius: 2px;
		transition: all 0.1s;
	}

	.window-control:hover {
		background: linear-gradient(to bottom, #f8f8f8, #e8e8e8);
	}

	.window-control:active {
		background: linear-gradient(to bottom, #e0e0e0, #d0d0d0);
	}

	.window-control.minimize {
		color: #666;
	}

	.window-control.maximize {
		color: #666;
		font-size: 10px;
	}

	.window-control.close {
		color: #d00;
		font-weight: bold;
	}

	.window-control.close:hover {
		background: #e00;
		color: white;
		border-color: #c00;
	}

	.window-content {
		height: calc(100% - 32px);
		overflow: auto;
		background: white;
	}

	.placeholder {
		padding: 20px;
		text-align: center;
		color: #666;
	}

	.resize-handle {
		position: absolute;
		bottom: 0;
		right: 0;
		width: 16px;
		height: 16px;
		cursor: nw-resize;
		background: linear-gradient(-45deg, transparent 0%, transparent 30%, #ccc 30%, #ccc 35%, transparent 35%, transparent 65%, #ccc 65%, #ccc 70%, transparent 70%);
	}

	.resize-handle:hover {
		background: linear-gradient(-45deg, transparent 0%, transparent 30%, #999 30%, #999 35%, transparent 35%, transparent 65%, #999 65%, #999 70%, transparent 70%);
	}
</style> 