<script lang="ts">
	interface Project {
		id: string;
		name: string;
		description: string;
		emoji: string;
		tech: string[];
		progress: number;
		status: 'active' | 'completed' | 'planned' | 'paused';
		githubUrl: string;
		liveUrl?: string;
	}

	const projects: Project[] = [
		{
			id: 'conspiracy-board',
			name: 'conspiracy-board',
			description: 'A TypeScript project exploring structured information linking and board logic.',
			emoji: '🧠',
			tech: ['TypeScript'],
			progress: 25,
			status: 'active',
			githubUrl: 'https://github.com/erdospeet11/conspiracy-board'
		},
		{
			id: 'Card Game in Lua',
			name: 'Card Game in Lua',
			description: 'A card game about combinating cards built with Lua and LÖVE.',
			emoji: '🃏',
			tech: ['Lua, LÖVE'],
			progress: 45,
			status: 'active',
			githubUrl: 'https://github.com/erdospeet11/lua-card-game'
		},
		{
			id: 'computer-portfolio',
			name: 'computer-portfolio',
			description: 'Personal portfolio site built with Svelte showcasing programming projects and skills.',
			emoji: '💻',
			tech: ['Svelte'],
			progress: 75,
			status: 'active',
			githubUrl: 'https://github.com/erdospeet11/computer-portfolio'
		},
		{
			id: 'cosmosjs-multiplayer',
			name: 'cosmosjs-multiplayer',
			description: 'Multiplayer JavaScript game with a websocket and threejs.',
			emoji: '🪐',
			tech: ['JavaScript'],
			progress: 25,
			status: 'paused',
			githubUrl: 'https://github.com/erdospeet11/cosmosjs-multiplayer'
		},
		{
			id: 'dbmc',
			name: 'dbmc',
			description: 'A C-based database management CLI tool for learning systems-level programming.',
			emoji: '💾',
			tech: ['C'],
			progress: 10,
			status: 'paused',
			githubUrl: 'https://github.com/erdospeet11/dbmc'
		},
		{
			id: 'homeinvasion',
			name: 'homeinvasion',
			description: 'A game project built with Godot and GDScript, featuring stealth and tension mechanics.',
			emoji: '🏠',
			tech: ['GDScript'],
			progress: 0,
			status: 'planned',
			githubUrl: 'https://github.com/erdospeet11/homeinvasion'
		},
		{
			id: 'the-void-engine',
			name: 'the-void-engine',
			description: 'A custom C++ game engine for experimental game development.',
			emoji: '🌌',
			tech: ['C++'],
			progress: 5,
			status: 'planned',
			githubUrl: 'https://github.com/erdospeet11/the-void-engine'
		},
	];

	function getStatusColor(status: string): string {
		switch (status) {
			case 'completed': return '#10B981';
			case 'active': return '#3B82F6';
			case 'paused': return '#F59E0B';
			case 'planned': return '#6B7280';
			default: return '#6B7280';
		}
	}

	function getStatusText(status: string): string {
		switch (status) {
			case 'completed': return 'Completed';
			case 'active': return 'In Progress';
			case 'paused': return 'Paused';
			case 'planned': return 'Planned';
			default: return 'Unknown';
		}
	}
</script>

<div class="projects-window">
	<div class="header">
		<h1>My Projects</h1>
		<p>A collection of my recent work and side projects with current progress</p>
	</div>

	<div class="projects-grid">
		{#each projects as project}
			<div class="project-card">
				<div class="project-image">
					{project.emoji}
				</div>
				<div class="project-content">
					<div class="project-header">
						<h3>{project.name}</h3>
						<span 
							class="status-badge" 
							style="background-color: {getStatusColor(project.status)}"
						>
							{getStatusText(project.status)}
						</span>
					</div>
					<p>{project.description}</p>
					
					<div class="progress-section">
						<div class="progress-header">
							<span class="progress-label">Progress</span>
							<span class="progress-percentage">{project.progress}%</span>
						</div>
						<div class="progress-bar">
							<div 
								class="progress-fill" 
								style="width: {project.progress}%; background-color: {getStatusColor(project.status)}"
							></div>
						</div>
					</div>
					
					<div class="tech-stack">
						{#each project.tech as tech}
							<span class="tech">{tech}</span>
						{/each}
					</div>
					
					<div class="project-links">
						{#if project.liveUrl}
							<button 
								class="link-btn primary" 
								on:click={() => window.open(project.liveUrl, '_blank')}
							>
								Live Demo
							</button>
						{:else}
							<button class="link-btn primary" disabled>
								Live Demo
							</button>
						{/if}
						<button 
							class="link-btn" 
							on:click={() => window.open(project.githubUrl, '_blank')}
						>
							GitHub
						</button>
					</div>
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	.projects-window {
		padding: 20px;
		height: 100%;
		overflow-y: auto;
		font-family: 'Space Mono', 'Consolas', 'Courier New', monospace;
	}

	.header {
		margin-bottom: 30px;
		text-align: center;
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

	.projects-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
		gap: 20px;
	}

	.project-card {
		background: white;
		border: 1px solid #ddd;
		border-radius: 8px;
		overflow: hidden;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
		transition: transform 0.2s, box-shadow 0.2s;
	}

	.project-card:hover {
		transform: translateY(-2px);
		box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
	}

	.project-image {
		height: 120px;
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 48px;
		color: white;
	}

	.project-content {
		padding: 20px;
	}

	.project-header {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		margin-bottom: 10px;
		gap: 10px;
	}

	.project-content h3 {
		margin: 0;
		color: #333;
		font-size: 18px;
		flex: 1;
	}

	.status-badge {
		color: white;
		padding: 2px 8px;
		border-radius: 12px;
		font-size: 10px;
		font-weight: 600;
		text-transform: uppercase;
		letter-spacing: 0.5px;
		white-space: nowrap;
	}

	.project-content p {
		margin: 0 0 15px 0;
		color: #666;
		line-height: 1.5;
		font-size: 14px;
	}

	.progress-section {
		margin-bottom: 15px;
	}

	.progress-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 6px;
	}

	.progress-label {
		font-size: 12px;
		font-weight: 600;
		color: #374151;
		text-transform: uppercase;
		letter-spacing: 0.5px;
	}

	.progress-percentage {
		font-size: 12px;
		font-weight: 700;
		color: #374151;
	}

	.progress-bar {
		width: 100%;
		height: 8px;
		background-color: #E5E7EB;
		border-radius: 4px;
		overflow: hidden;
	}

	.progress-fill {
		height: 100%;
		transition: width 0.3s ease, background-color 0.3s ease;
		border-radius: 4px;
	}

	.tech-stack {
		margin-bottom: 15px;
		display: flex;
		flex-wrap: wrap;
		gap: 6px;
	}

	.tech {
		background: #f0f2f5;
		color: #333;
		padding: 4px 8px;
		border-radius: 12px;
		font-size: 12px;
		font-weight: 500;
	}

	.project-links {
		display: flex;
		gap: 10px;
	}

	.link-btn {
		flex: 1;
		padding: 8px 16px;
		border: 1px solid #667eea;
		background: transparent;
		color: #667eea;
		border-radius: 4px;
		cursor: pointer;
		font-size: 12px;
		font-weight: 500;
		transition: all 0.2s;
	}

	.link-btn:hover:not(:disabled) {
		background: #667eea;
		color: white;
	}

	.link-btn.primary {
		background: #667eea;
		color: white;
	}

	.link-btn.primary:hover:not(:disabled) {
		background: #5a6fd8;
	}

	.link-btn:disabled {
		opacity: 0.5;
		cursor: not-allowed;
		background: #f3f4f6;
		color: #9ca3af;
		border-color: #d1d5db;
	}
</style> 