<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	export let currentTime: string;
	export let openWindows: Array<{
		id: string;
		title: string;
		component: string;
		minimized: boolean;
	}>;
	export let startMenuOpen: boolean;

	const dispatch = createEventDispatcher();

	const startMenuItems = [
		{ id: 'about', name: 'About Me', icon: '👤', component: 'AboutWindow' },
		{ id: 'projects', name: 'Projects', icon: '📁', component: 'ProjectsWindow' },
		{ id: 'cards', name: 'Card Game', icon: '🃏', component: 'CardGameWindow' },
		{ id: 'contact', name: 'Contact', icon: '📧', component: 'ContactWindow' },
		{ id: 'resume', name: 'Resume', icon: '📄', component: 'ResumeWindow' },
		{ id: 'wallpaper', name: 'Wallpaper', icon: '🖼️', component: 'WallpaperWindow' },
		{ id: 'terminal', name: 'Terminal', icon: '⌨️', component: 'TerminalWindow' }
	];

	function handleStartClick(event: MouseEvent) {
		event.stopPropagation();
		dispatch('toggleStart');
	}

	function handleTaskClick(windowId: string, event: MouseEvent) {
		event.stopPropagation();
		dispatch('taskClick', windowId);
	}

	function handleStartMenuClick(item: any, event: MouseEvent) {
		event.stopPropagation();
		dispatch('openApp', {
			id: item.id,
			title: item.name,
			component: item.component
		});
		dispatch('toggleStart');
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="taskbar">
	<!-- Start Button -->
	<button class="start-button" class:active={startMenuOpen} on:click={handleStartClick}>
		<span class="windows-icon">🚀</span>
		Start
	</button>

	<!-- Task List -->
	<div class="task-list">
		{#each openWindows as window}
			<button 
				class="task-button" 
				class:minimized={window.minimized}
				on:click={(e) => handleTaskClick(window.id, e)}
			>
				{window.title}
			</button>
		{/each}
	</div>

	<!-- System Tray -->
	<div class="system-tray">
		<div class="time">{currentTime}</div>
	</div>

	<!-- Start Menu -->
	{#if startMenuOpen}
		<div class="start-menu" on:click={(e) => e.stopPropagation()}>
			<div class="start-menu-header">
				<div class="user-info">
					<div class="user-avatar">👤</div>
					<div class="user-name">Portfolio User</div>
				</div>
			</div>
			<div class="start-menu-items">
				{#each startMenuItems as item}
					<div 
						class="start-menu-item" 
						on:click={(e) => handleStartMenuClick(item, e)}
					>
						<span class="menu-icon">{item.icon}</span>
						<span class="menu-name">{item.name}</span>
					</div>
				{/each}
			</div>
			<div class="start-menu-footer">
				<button class="power-button">⏻ Power</button>
			</div>
		</div>
	{/if}
</div>

<style>
	.taskbar {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		height: 48px;
		background: rgba(0, 0, 0, 0.8);
		backdrop-filter: blur(10px);
		border-top: 1px solid rgba(255, 255, 255, 0.1);
		display: flex;
		align-items: center;
		padding: 0 8px;
		z-index: 1000;
	}

	.start-button {
		display: flex;
		align-items: center;
		gap: 8px;
		padding: 8px 16px;
		background: transparent;
		border: none;
		color: white;
		cursor: pointer;
		border-radius: 4px;
		font-size: 14px;
		transition: background-color 0.2s;
	}

	.start-button:hover, .start-button.active {
		background: rgba(255, 255, 255, 0.1);
	}

	.windows-icon {
		font-size: 16px;
	}

	.task-list {
		flex: 1;
		display: flex;
		gap: 4px;
		margin-left: 8px;
		overflow-x: auto;
	}

	.task-button {
		min-width: 160px;
		max-width: 200px;
		padding: 8px 12px;
		background: rgba(255, 255, 255, 0.1);
		border: 1px solid rgba(255, 255, 255, 0.2);
		color: white;
		cursor: pointer;
		border-radius: 4px;
		font-size: 12px;
		text-overflow: ellipsis;
		overflow: hidden;
		white-space: nowrap;
		transition: all 0.2s;
	}

	.task-button:hover {
		background: rgba(255, 255, 255, 0.2);
	}

	.task-button.minimized {
		opacity: 0.6;
	}

	.system-tray {
		display: flex;
		align-items: center;
		gap: 8px;
		margin-left: 8px;
	}

	.time {
		color: white;
		font-size: 13px;
		padding: 8px 12px;
		border-radius: 4px;
		cursor: pointer;
		transition: background-color 0.2s;
	}

	.time:hover {
		background: rgba(255, 255, 255, 0.1);
	}

	.start-menu {
		position: fixed;
		bottom: 48px;
		left: 0;
		width: 320px;
		background: rgba(32, 32, 32, 0.95);
		backdrop-filter: blur(20px);
		border: 1px solid rgba(255, 255, 255, 0.1);
		border-radius: 8px 8px 0 0;
		box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.3);
		overflow: hidden;
	}

	.start-menu-header {
		padding: 16px;
		border-bottom: 1px solid rgba(255, 255, 255, 0.1);
	}

	.user-info {
		display: flex;
		align-items: center;
		gap: 12px;
	}

	.user-avatar {
		font-size: 24px;
		width: 40px;
		height: 40px;
		display: flex;
		align-items: center;
		justify-content: center;
		background: rgba(0, 120, 212, 0.2);
		border-radius: 50%;
	}

	.user-name {
		color: white;
		font-size: 14px;
		font-weight: 500;
	}

	.start-menu-items {
		padding: 8px 0;
	}

	.start-menu-item {
		display: flex;
		align-items: center;
		gap: 12px;
		padding: 12px 16px;
		color: white;
		cursor: pointer;
		transition: background-color 0.2s;
	}

	.start-menu-item:hover {
		background: rgba(255, 255, 255, 0.1);
	}

	.menu-icon {
		font-size: 20px;
		width: 24px;
		text-align: center;
	}

	.menu-name {
		font-size: 14px;
	}

	.start-menu-footer {
		padding: 8px 16px;
		border-top: 1px solid rgba(255, 255, 255, 0.1);
	}

	.power-button {
		width: 100%;
		padding: 8px 12px;
		background: transparent;
		border: 1px solid rgba(255, 255, 255, 0.2);
		color: white;
		cursor: pointer;
		border-radius: 4px;
		font-size: 12px;
		transition: background-color 0.2s;
	}

	.power-button:hover {
		background: rgba(255, 255, 255, 0.1);
	}
</style> 