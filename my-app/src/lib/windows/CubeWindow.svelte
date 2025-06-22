<script lang="ts">
	import { onMount } from 'svelte';
	import * as THREE from 'three';

	let canvas: HTMLCanvasElement;
	let animationId: number;
	let scene: THREE.Scene;
	let camera: THREE.PerspectiveCamera;
	let renderer: THREE.WebGLRenderer;
	let cube: THREE.Mesh;
	let isAnimating = true;
	let animationSpeed = 1;
	let cubeColor = '#ff6b6b';
	let wireframe = false;

	onMount(() => {
		initThreeJS();
		animate();

		return () => {
			if (animationId) {
				cancelAnimationFrame(animationId);
			}
			if (renderer) {
				renderer.dispose();
			}
		};
	});

	function initThreeJS() {
		// Scene
		scene = new THREE.Scene();
		scene.background = new THREE.Color(0x222222);

		// Camera
		camera = new THREE.PerspectiveCamera(75, canvas.clientWidth / canvas.clientHeight, 0.1, 1000);
		camera.position.z = 5;

		// Renderer
		renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
		renderer.setSize(canvas.clientWidth, canvas.clientHeight);
		renderer.setPixelRatio(window.devicePixelRatio);

		// Cube geometry and material
		const geometry = new THREE.BoxGeometry(2, 2, 2);
		const material = new THREE.MeshPhongMaterial({ 
			color: cubeColor,
			wireframe: wireframe
		});
		cube = new THREE.Mesh(geometry, material);
		scene.add(cube);

		// Lighting
		const ambientLight = new THREE.AmbientLight(0x404040, 0.6);
		scene.add(ambientLight);

		const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
		directionalLight.position.set(5, 5, 5);
		scene.add(directionalLight);

		// Handle window resize
		const handleResize = () => {
			if (canvas && renderer && camera) {
				const width = canvas.clientWidth;
				const height = canvas.clientHeight;
				
				camera.aspect = width / height;
				camera.updateProjectionMatrix();
				renderer.setSize(width, height);
			}
		};

		window.addEventListener('resize', handleResize);
	}

	function animate() {
		animationId = requestAnimationFrame(animate);

		if (isAnimating && cube) {
			cube.rotation.x += 0.01 * animationSpeed;
			cube.rotation.y += 0.01 * animationSpeed;
		}

		if (renderer && scene && camera) {
			renderer.render(scene, camera);
		}
	}

	function toggleAnimation() {
		isAnimating = !isAnimating;
	}

	function updateCubeColor() {
		if (cube && cube.material instanceof THREE.MeshPhongMaterial) {
			cube.material.color.setHex(parseInt(cubeColor.replace('#', ''), 16));
		}
	}

	function toggleWireframe() {
		wireframe = !wireframe;
		if (cube && cube.material instanceof THREE.MeshPhongMaterial) {
			cube.material.wireframe = wireframe;
		}
	}

	function resetCube() {
		if (cube) {
			cube.rotation.set(0, 0, 0);
		}
	}
</script>

<div class="cube-window">
	<div class="controls">
		<div class="control-group">
			<button on:click={toggleAnimation} class="control-btn">
				{isAnimating ? '⏸️' : '▶️'} {isAnimating ? 'Pause' : 'Play'}
			</button>
			<button on:click={resetCube} class="control-btn">🔄 Reset</button>
			<button on:click={toggleWireframe} class="control-btn">
				{wireframe ? '🔲' : '⬜'} {wireframe ? 'Solid' : 'Wireframe'}
			</button>
		</div>
		
		<div class="control-group">
			<label for="speed">Speed:</label>
			<input 
				type="range" 
				id="speed"
				min="0.1" 
				max="3" 
				step="0.1" 
				bind:value={animationSpeed}
				class="slider"
			/>
			<span class="value">{animationSpeed.toFixed(1)}</span>
		</div>

		<div class="control-group">
			<label for="color">Color:</label>
			<input 
				type="color" 
				id="color"
				bind:value={cubeColor}
				on:input={updateCubeColor}
				class="color-picker"
			/>
		</div>
	</div>

	<div class="canvas-container">
		<canvas bind:this={canvas}></canvas>
	</div>
</div>

<style>
	.cube-window {
		height: 100%;
		display: flex;
		flex-direction: column;
		font-family: 'Space Mono', 'Consolas', 'Courier New', monospace;
		background: #1a1a1a;
		color: #ffffff;
	}

	.controls {
		padding: 15px;
		background: #2d2d2d;
		border-bottom: 1px solid #444;
		display: flex;
		flex-wrap: wrap;
		gap: 15px;
		align-items: center;
	}

	.control-group {
		display: flex;
		align-items: center;
		gap: 8px;
	}

	.control-btn {
		background: #4a4a4a;
		color: white;
		border: none;
		padding: 8px 12px;
		border-radius: 4px;
		cursor: pointer;
		font-family: inherit;
		font-size: 12px;
		transition: background-color 0.2s;
	}

	.control-btn:hover {
		background: #5a5a5a;
	}

	.control-btn:active {
		background: #3a3a3a;
	}

	label {
		font-size: 12px;
		color: #ccc;
	}

	.slider {
		width: 80px;
		height: 4px;
		background: #555;
		outline: none;
		border-radius: 2px;
	}

	.slider::-webkit-slider-thumb {
		appearance: none;
		width: 16px;
		height: 16px;
		background: #ff6b6b;
		cursor: pointer;
		border-radius: 50%;
	}

	.slider::-moz-range-thumb {
		width: 16px;
		height: 16px;
		background: #ff6b6b;
		cursor: pointer;
		border-radius: 50%;
		border: none;
	}

	.value {
		font-size: 12px;
		color: #fff;
		min-width: 25px;
	}

	.color-picker {
		width: 30px;
		height: 25px;
		border: none;
		border-radius: 4px;
		cursor: pointer;
		background: none;
	}

	.canvas-container {
		flex: 1;
		position: relative;
		overflow: hidden;
	}

	canvas {
		width: 100%;
		height: 100%;
		display: block;
	}
</style> 