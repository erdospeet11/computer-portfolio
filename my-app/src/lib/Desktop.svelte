<script lang="ts">
	import { onMount } from 'svelte';
	import Taskbar from './Taskbar.svelte';
	import DesktopIcon from './DesktopIcon.svelte';
	import Window from './Window.svelte';

	let currentTime = '';
	let openWindows: Array<{
		id: string;
		title: string;
		component: string;
		x: number;
		y: number;
		width: number;
		height: number;
		minimized: boolean;
		zIndex: number;
		props?: any;
	}> = [];
	
	let maxZIndex = 100;
	let startMenuOpen = false;
	let currentWallpaper = 'nature';
	let selectedIconId: string | null = null;

	let desktopIcons = [
		{ id: 'about', name: 'About Me', icon: '👤', x: 50, y: 50 },
		{ id: 'projects', name: 'Projects', icon: '📁', x: 50, y: 150 },
		{ id: 'cards', name: 'Card Game', icon: '🃏', x: 50, y: 250 },
		{ id: 'contact', name: 'Contact', icon: '📧', x: 50, y: 350 },
		{ id: 'resume', name: 'Resume', icon: '📕', x: 50, y: 450 },
		{ id: 'wallpaper', name: 'Wallpaper', icon: '🖼️', x: 50, y: 550 },
		{ id: 'terminal', name: 'Terminal', icon: '⌨️', x: 150, y: 50 },
		{ id: 'cube', name: '3D Cube', icon: '🎲', x: 150, y: 150 },
		{ id: 'explorer', name: 'File Explorer', icon: '📂', x: 150, y: 250 },
		{ id: 'image1', name: 'Landscape.svg', icon: '🖼️', x: 250, y: 50 },
		{ id: 'image2', name: 'Abstract.svg', icon: '🎨', x: 250, y: 150 },
		{ id: 'image3', name: 'Geometric.svg', icon: '🔷', x: 250, y: 250 },
		{ id: 'secret', name: 'secret.txt', icon: '📄', x: 1200, y: 600 }
	];

	onMount(() => {
		const updateTime = () => {
			const now = new Date();
			currentTime = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
		};
		updateTime();
		const timeInterval = setInterval(updateTime, 1000);

		const secretIcon = desktopIcons.find(icon => icon.id === 'secret');
		if (secretIcon) {
			secretIcon.x = window.innerWidth - 130;
			secretIcon.y = window.innerHeight - 180;
			desktopIcons = [...desktopIcons];
		}

		return () => clearInterval(timeInterval);
	});

	function openWindow(id: string, title: string, component: string, props: any = {}) {
		const existingWindow = openWindows.find(w => w.id === id);
		if (existingWindow) {
			existingWindow.minimized = false;
			existingWindow.zIndex = ++maxZIndex;
			openWindows = [...openWindows];
			return;
		}

		const newWindow = {
			id,
			title,
			component,
			x: 100 + openWindows.length * 30,
			y: 100 + openWindows.length * 30,
			width: 600,
			height: 400,
			minimized: false,
			zIndex: ++maxZIndex,
			props
		};

		openWindows = [...openWindows, newWindow];
	}

	function closeWindow(id: string) {
		openWindows = openWindows.filter(w => w.id !== id);
	}

	function minimizeWindow(id: string) {
		const window = openWindows.find(w => w.id === id);
		if (window) {
			window.minimized = true;
			openWindows = [...openWindows];
		}
	}

	function bringToFront(id: string) {
		const window = openWindows.find(w => w.id === id);
		if (window) {
			window.zIndex = ++maxZIndex;
			openWindows = [...openWindows];
		}
	}

	function handleIconDoubleClick(iconId: string) {
		const iconMap: { [key: string]: { title: string; component: string; props?: any } } = {
			about: { title: 'About Me', component: 'AboutWindow' },
			projects: { title: 'Projects', component: 'ProjectsWindow' },
			cards: { title: 'Card Game', component: 'CardGameWindow' },
			contact: { title: 'Contact', component: 'ContactWindow' },
			resume: { title: 'Resume', component: 'ResumeWindow' },
			wallpaper: { title: 'Wallpaper Settings', component: 'WallpaperWindow' },
			terminal: { title: 'Command Prompt', component: 'TerminalWindow' },
			cube: { title: '3D Cube Viewer', component: 'CubeWindow' },
			explorer: { title: 'File Explorer', component: 'FileExplorerWindow' },
			image1: { 
				title: 'Landscape.svg', 
				component: 'ImageWindow', 
				props: { imageSrc: '/images/sample1.svg', imageName: 'Landscape.svg' }
			},
			image2: { 
				title: 'Abstract.svg', 
				component: 'ImageWindow', 
				props: { imageSrc: '/images/sample2.svg', imageName: 'Abstract.svg' }
			},
			image3: { 
				title: 'Geometric.svg', 
				component: 'ImageWindow', 
				props: { imageSrc: '/images/sample3.svg', imageName: 'Geometric.svg' }
			},
			secret: {
				title: 'secret.txt',
				component: 'TextWindow',
				props: { textContent: 'If you find this secret, go to the terminal and type "cat secret.txt" to see the secret.', fileName: 'secret.txt', readonly: true }
			}
		};

		const config = iconMap[iconId];
		if (config) {
			openWindow(iconId, config.title, config.component, config.props);
		}
	}

	function toggleStartMenu() {
		startMenuOpen = !startMenuOpen;
	}

	function handleDesktopClick() {
		startMenuOpen = false;
		selectedIconId = null;
	}

	function handleIconClick(iconId: string) {
		selectedIconId = iconId;
	}

	function handleWallpaperChange(event: CustomEvent<string>) {
		currentWallpaper = event.detail;
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="desktop" on:click={handleDesktopClick}>
	<!-- Desktop Background -->
	<div class="desktop-background" class:nature={currentWallpaper === 'nature'} class:space={currentWallpaper === 'space'} class:ocean={currentWallpaper === 'ocean'} class:sunset={currentWallpaper === 'sunset'} class:forest={currentWallpaper === 'forest'}></div>
	
	<!-- Desktop Icons -->
	{#each desktopIcons as icon}
		<DesktopIcon
			{...icon}
			selected={selectedIconId === icon.id}
			on:doubleclick={() => handleIconDoubleClick(icon.id)}
			on:click={() => handleIconClick(icon.id)}
		/>
	{/each}

	<!-- Open Windows -->
	{#each openWindows as window (window.id)}
		{#if !window.minimized}
			<Window
				{...window}
				props={window.props || {}}
				on:close={() => closeWindow(window.id)}
				on:minimize={() => minimizeWindow(window.id)}
				on:focus={() => bringToFront(window.id)}
				on:wallpaperChange={handleWallpaperChange}
				bind:x={window.x}
				bind:y={window.y}
				bind:width={window.width}
				bind:height={window.height}
			/>
		{/if}
	{/each}

	<!-- Taskbar -->
	<Taskbar
		{currentTime}
		{openWindows}
		{startMenuOpen}
		on:toggleStart={toggleStartMenu}
		on:taskClick={(e: CustomEvent<string>) => {
			const window = openWindows.find(w => w.id === e.detail);
			if (window) {
				window.minimized = !window.minimized;
				if (!window.minimized) {
					window.zIndex = ++maxZIndex;
				}
				openWindows = [...openWindows];
			}
		}}
		on:openApp={(e: CustomEvent<{id: string, title: string, component: string, props?: any}>) => {
			const { id, title, component, props } = e.detail;
			openWindow(id, title, component, props);
		}}
	/>
</div>

<style>
	.desktop {
		position: relative;
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		cursor: default;
		user-select: none;
	}

	.desktop-background {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		position: relative;
		transition: all 0.5s ease-in-out;
	}

	/* Nature wallpaper (default) */
	.desktop-background.nature {
		background: 
			/* Sky gradient */
			linear-gradient(180deg, #87CEEB 0%, #98D8E8 30%, #F0E68C 70%, #FFA07A 100%),
			/* Mountain layers */
			linear-gradient(180deg, transparent 0%, transparent 60%, #2F4F4F 60%, #2F4F4F 70%, transparent 70%),
			linear-gradient(180deg, transparent 0%, transparent 65%, #4682B4 65%, #4682B4 75%, transparent 75%),
			linear-gradient(180deg, transparent 0%, transparent 70%, #228B22 70%, #228B22 80%, transparent 80%),
			/* Foreground trees */
			linear-gradient(180deg, transparent 0%, transparent 75%, #006400 75%, #006400 85%, transparent 85%),
			linear-gradient(180deg, transparent 0%, transparent 80%, #2E8B57 80%, #2E8B57 90%, transparent 90%),
			/* Ground */
			linear-gradient(180deg, transparent 0%, transparent 85%, #8FBC8F 85%, #8FBC8F 100%);
		background-size: 
			100% 100%,
			100% 100%,
			100% 100%, 
			100% 100%,
			100% 100%,
			100% 100%,
			100% 100%;
	}

	/* Space wallpaper */
	.desktop-background.space {
		background: 
			radial-gradient(ellipse at center, #0a0a23 0%, #000000 70%),
			radial-gradient(circle at 20% 80%, #120458 0%, transparent 50%),
			radial-gradient(circle at 80% 20%, #ffd700 0%, transparent 50%),
			radial-gradient(circle at 40% 40%, #ffffff 0%, transparent 50%);
		background-size: 100% 100%, 50% 50%, 25% 25%, 15% 15%;
	}

	/* Ocean wallpaper */
	.desktop-background.ocean {
		background: 
			linear-gradient(180deg, #87CEEB 0%, #4682B4 30%, #1e90ff 60%, #006994 100%),
			linear-gradient(180deg, transparent 0%, transparent 70%, #20B2AA 70%, #20B2AA 85%, transparent 85%),
			linear-gradient(180deg, transparent 0%, transparent 80%, #008B8B 80%, #008B8B 95%, transparent 95%);
	}

	/* Sunset wallpaper */
	.desktop-background.sunset {
		background: 
			linear-gradient(180deg, #FF6B35 0%, #F7931E 25%, #FFD23F 50%, #FFA500 75%, #FF4500 100%),
			linear-gradient(180deg, transparent 0%, transparent 70%, #8B4513 70%, #8B4513 80%, transparent 80%),
			linear-gradient(180deg, transparent 0%, transparent 85%, #2F4F4F 85%, #2F4F4F 100%, transparent 100%);
	}

	/* Forest wallpaper */
	.desktop-background.forest {
		background: 
			linear-gradient(180deg, #228B22 0%, #32CD32 30%, #006400 70%, #2F4F4F 100%),
			linear-gradient(180deg, transparent 0%, transparent 50%, #8FBC8F 50%, #8FBC8F 70%, transparent 70%),
			linear-gradient(180deg, transparent 0%, transparent 70%, #556B2F 70%, #556B2F 90%, transparent 90%);
	}

	.desktop-background.nature::before {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background-image: 
			/* Clouds */
			radial-gradient(ellipse 120px 80px at 20% 20%, rgba(255, 255, 255, 0.8) 0%, transparent 50%),
			radial-gradient(ellipse 100px 60px at 80% 15%, rgba(255, 255, 255, 0.7) 0%, transparent 50%),
			radial-gradient(ellipse 140px 90px at 60% 25%, rgba(255, 255, 255, 0.6) 0%, transparent 50%),
			radial-gradient(ellipse 80px 50px at 15% 30%, rgba(255, 255, 255, 0.5) 0%, transparent 50%),
			radial-gradient(ellipse 110px 70px at 85% 35%, rgba(255, 255, 255, 0.4) 0%, transparent 50%),
			/* Tree texture */
			radial-gradient(circle 3px at 25% 78%, rgba(34, 139, 34, 0.8) 0%, transparent 50%),
			radial-gradient(circle 2px at 75% 82%, rgba(46, 139, 87, 0.7) 0%, transparent 50%),
			radial-gradient(circle 4px at 45% 79%, rgba(0, 100, 0, 0.6) 0%, transparent 50%),
			radial-gradient(circle 2px at 65% 85%, rgba(34, 139, 34, 0.5) 0%, transparent 50%),
			/* Mountain snow caps */
			radial-gradient(ellipse 50px 20px at 30% 62%, rgba(255, 255, 255, 0.9) 0%, transparent 70%),
			radial-gradient(ellipse 40px 15px at 70% 67%, rgba(255, 255, 255, 0.8) 0%, transparent 70%);
		animation: gentle-sway 20s ease-in-out infinite;
	}

	.desktop-background.space::before {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background-image: 
			/* Stars */
			radial-gradient(circle 1px at 25% 25%, white 0%, transparent 50%),
			radial-gradient(circle 1px at 75% 25%, white 0%, transparent 50%),
			radial-gradient(circle 1px at 50% 50%, white 0%, transparent 50%),
			radial-gradient(circle 1px at 80% 80%, white 0%, transparent 50%),
			radial-gradient(circle 1px at 20% 80%, white 0%, transparent 50%),
			radial-gradient(circle 2px at 90% 30%, #ffd700 0%, transparent 50%),
			radial-gradient(circle 1px at 10% 60%, white 0%, transparent 50%);
		animation: twinkle 3s ease-in-out infinite;
	}

	@keyframes gentle-sway {
		0%, 100% { transform: translateX(0px); }
		50% { transform: translateX(2px); }
	}

	@keyframes twinkle {
		0%, 100% { opacity: 1; }
		50% { opacity: 0.5; }
	}
</style> 