<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	const wallpapers = [
		{
			id: 'nature',
			name: 'Mountain Nature',
			description: 'Peaceful mountain landscape with forests and sky',
			preview: 'linear-gradient(180deg, #87CEEB 0%, #228B22 70%, #8FBC8F 100%)'
		},
		{
			id: 'space',
			name: 'Deep Space',
			description: 'Dark cosmic background with twinkling stars',
			preview: 'radial-gradient(ellipse at center, #0a0a23 0%, #000000 70%)'
		},
		{
			id: 'ocean',
			name: 'Ocean Blue',
			description: 'Serene ocean waves with blue gradient sky',
			preview: 'linear-gradient(180deg, #87CEEB 0%, #1e90ff 60%, #006994 100%)'
		},
		{
			id: 'sunset',
			name: 'Golden Sunset',
			description: 'Warm sunset colors with orange and golden hues',
			preview: 'linear-gradient(180deg, #FF6B35 0%, #FFD23F 50%, #FF4500 100%)'
		},
		{
			id: 'forest',
			name: 'Dense Forest',
			description: 'Lush green forest with varying shades of green',
			preview: 'linear-gradient(180deg, #228B22 0%, #32CD32 30%, #006400 100%)'
		}
	];

	let selectedWallpaper = 'nature';

	function selectWallpaper(wallpaperId: string) {
		selectedWallpaper = wallpaperId;
	}

	function applyWallpaper() {
		dispatch('wallpaperChange', selectedWallpaper);
	}

	function previewWallpaper(wallpaperId: string) {
		dispatch('wallpaperChange', wallpaperId);
	}

	function resetPreview() {
		// For now, just keep the selected wallpaper
	}
</script>

<div class="wallpaper-window">
	<div class="header">
		<h1>🖼️ Wallpaper Settings</h1>
		<p>Choose your desktop background</p>
	</div>

	<div class="content">
		<div class="wallpaper-grid">
			{#each wallpapers as wallpaper}
				<!-- svelte-ignore a11y-click-events-have-key-events -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div 
					class="wallpaper-card"
					class:selected={selectedWallpaper === wallpaper.id}
					on:click={() => selectWallpaper(wallpaper.id)}
					on:mouseenter={() => previewWallpaper(wallpaper.id)}
					on:mouseleave={resetPreview}
				>
					<div 
						class="wallpaper-preview"
						style="background: {wallpaper.preview}"
					>
						<div class="preview-overlay">
							{#if selectedWallpaper === wallpaper.id}
								<div class="selected-indicator">✓</div>
							{/if}
						</div>
					</div>
					<div class="wallpaper-info">
						<h3>{wallpaper.name}</h3>
						<p>{wallpaper.description}</p>
					</div>
				</div>
			{/each}
		</div>

		<div class="actions">
			<div class="current-selection">
				<strong>Selected:</strong> {wallpapers.find(w => w.id === selectedWallpaper)?.name}
			</div>
			<button class="apply-btn" on:click={applyWallpaper}>
				🎨 Apply Wallpaper
			</button>
		</div>

		<div class="info-section">
			<h2>💡 Tips</h2>
			<ul>
				<li>Hover over wallpapers to preview them instantly</li>
				<li>Click to select, then press "Apply Wallpaper"</li>
				<li>Changes apply immediately to your desktop</li>
				<li>Your selection is saved for the current session</li>
			</ul>
		</div>
	</div>
</div>

<style>
	.wallpaper-window {
		padding: 20px;
		height: 100%;
		overflow-y: auto;
		font-family: 'Space Mono', 'Consolas', 'Courier New', monospace;
		background: #f8f9fa;
	}

	.header {
		text-align: center;
		margin-bottom: 30px;
		padding-bottom: 20px;
		border-bottom: 2px solid #dee2e6;
	}

	.header h1 {
		margin: 0 0 10px 0;
		color: #333;
		font-size: 28px;
	}

	.header p {
		margin: 0;
		color: #666;
		font-size: 16px;
	}

	.wallpaper-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
		gap: 20px;
		margin-bottom: 30px;
	}

	.wallpaper-card {
		background: white;
		border: 2px solid #dee2e6;
		border-radius: 12px;
		overflow: hidden;
		cursor: pointer;
		transition: all 0.3s ease;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
	}

	.wallpaper-card:hover {
		transform: translateY(-2px);
		box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
		border-color: #667eea;
	}

	.wallpaper-card.selected {
		border-color: #667eea;
		box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.2);
	}

	.wallpaper-preview {
		height: 120px;
		position: relative;
		background-size: cover;
		background-position: center;
	}

	.preview-overlay {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: rgba(0, 0, 0, 0.1);
		display: flex;
		align-items: center;
		justify-content: center;
		opacity: 0;
		transition: opacity 0.2s;
	}

	.wallpaper-card.selected .preview-overlay {
		opacity: 1;
	}

	.selected-indicator {
		background: #667eea;
		color: white;
		width: 40px;
		height: 40px;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 24px;
		font-weight: bold;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
	}

	.wallpaper-info {
		padding: 15px;
	}

	.wallpaper-info h3 {
		margin: 0 0 8px 0;
		color: #333;
		font-size: 16px;
	}

	.wallpaper-info p {
		margin: 0;
		color: #666;
		font-size: 13px;
		line-height: 1.4;
	}

	.actions {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 20px;
		background: white;
		border-radius: 8px;
		margin-bottom: 20px;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
	}

	.current-selection {
		color: #333;
		font-size: 14px;
	}

	.apply-btn {
		padding: 12px 24px;
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		color: white;
		border: none;
		border-radius: 6px;
		cursor: pointer;
		font-size: 14px;
		font-weight: 600;
		transition: all 0.2s;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
	}

	.apply-btn:hover {
		transform: translateY(-1px);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
	}

	.apply-btn:active {
		transform: translateY(0);
	}

	.info-section {
		background: white;
		padding: 20px;
		border-radius: 8px;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
	}

	.info-section h2 {
		margin: 0 0 15px 0;
		color: #333;
		font-size: 18px;
	}

	.info-section ul {
		margin: 0;
		padding-left: 20px;
		color: #666;
	}

	.info-section li {
		margin-bottom: 8px;
		line-height: 1.4;
	}
</style> 