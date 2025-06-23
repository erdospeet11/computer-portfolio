<script lang="ts">
	export let imageSrc: string = '';
	export let imageName: string = 'Image';

	let imageElement: HTMLImageElement;
	let imageLoaded = false;
	let imageError = false;
	let zoom = 1;
	let maxZoom = 3;
	let minZoom = 0.1;

	function handleImageLoad() {
		imageLoaded = true;
		imageError = false;
	}

	function handleImageError() {
		imageError = true;
		imageLoaded = false;
	}

	function zoomIn() {
		zoom = Math.min(maxZoom, zoom * 1.2);
	}

	function zoomOut() {
		zoom = Math.max(minZoom, zoom / 1.2);
	}

	function resetZoom() {
		zoom = 1;
	}

	function fitToWindow() {
		if (imageElement) {
			const containerWidth = imageElement.parentElement?.clientWidth || 600;
			const containerHeight = (imageElement.parentElement?.clientHeight || 400) - 50;
			const scaleX = containerWidth / imageElement.naturalWidth;
			const scaleY = containerHeight / imageElement.naturalHeight;
			zoom = Math.min(scaleX, scaleY, 1);
		}
	}
</script>

<div class="image-window">
	<div class="image-toolbar">
		<div class="toolbar-group">
			<button class="toolbar-btn" on:click={zoomOut} title="Zoom Out">🔍−</button>
			<span class="zoom-level">{Math.round(zoom * 100)}%</span>
			<button class="toolbar-btn" on:click={zoomIn} title="Zoom In">🔍+</button>
		</div>
		<div class="toolbar-group">
			<button class="toolbar-btn" on:click={resetZoom} title="Actual Size">1:1</button>
			<button class="toolbar-btn" on:click={fitToWindow} title="Fit to Window">📐</button>
		</div>
		<div class="image-name">{imageName}</div>
	</div>

	<div class="image-container">
		{#if imageError}
			<div class="error-message">
				<div class="error-icon">❌</div>
				<p>Failed to load image</p>
				<p class="error-path">{imageSrc}</p>
			</div>
		{:else if !imageLoaded}
			<div class="loading-message">
				<div class="loading-spinner">⏳</div>
				<p>Loading image...</p>
			</div>
		{/if}

		<img
			bind:this={imageElement}
			src={imageSrc}
			alt={imageName}
			class="main-image"
			class:loaded={imageLoaded}
			style="transform: scale({zoom}); display: {imageLoaded ? 'block' : 'none'};"
			on:load={handleImageLoad}
			on:error={handleImageError}
			draggable="false"
		/>
	</div>
</div>

<style>
	.image-window {
		height: 100%;
		display: flex;
		flex-direction: column;
		background: #f5f5f5;
	}

	.image-toolbar {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 8px 12px;
		background: #e8e8e8;
		border-bottom: 1px solid #ccc;
		flex-shrink: 0;
	}

	.toolbar-group {
		display: flex;
		align-items: center;
		gap: 8px;
	}

	.toolbar-btn {
		padding: 4px 8px;
		border: 1px solid #ccc;
		background: white;
		border-radius: 3px;
		cursor: pointer;
		font-size: 12px;
		transition: background-color 0.2s;
	}

	.toolbar-btn:hover {
		background: #f0f0f0;
	}

	.toolbar-btn:active {
		background: #e0e0e0;
	}

	.zoom-level {
		font-size: 12px;
		color: #666;
		min-width: 40px;
		text-align: center;
	}

	.image-name {
		font-size: 12px;
		color: #666;
		font-weight: 500;
	}

	.image-container {
		flex: 1;
		overflow: auto;
		display: flex;
		align-items: center;
		justify-content: center;
		background: #fff;
		position: relative;
	}

	.main-image {
		max-width: none;
		max-height: none;
		transition: transform 0.2s ease;
		transform-origin: center;
	}

	.main-image.loaded {
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
	}

	.loading-message,
	.error-message {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 10px;
		color: #666;
		text-align: center;
	}

	.loading-spinner {
		font-size: 24px;
		animation: spin 1s linear infinite;
	}

	.error-icon {
		font-size: 24px;
	}

	.error-path {
		font-family: monospace;
		font-size: 11px;
		color: #999;
		word-break: break-all;
		max-width: 300px;
	}

	@keyframes spin {
		from { transform: rotate(0deg); }
		to { transform: rotate(360deg); }
	}
</style> 