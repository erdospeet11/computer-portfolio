<div class="projects-window">
	<div class="header">
		<h1>My Projects</h1>
		<p>A collection of my recent work and side projects</p>
	</div>

	<div class="projects-grid">
		<div class="project-card">
		  <div class="project-image">🧠</div>
		  <div class="project-content">
			<h3>conspiracy-board</h3>
			<p>A TypeScript project exploring structured information linking and board logic.</p>
			<div class="tech-stack">
			  <span class="tech">TypeScript</span>
			</div>
			<div class="project-links">
			  <button class="link-btn">Live Demo</button>
			  <button onclick={() => window.open('https://github.com/erdospeet11/conspiracy-board', '_blank')} class="link-btn">GitHub</button>
			</div>
		  </div>
		</div>
	  
		<div class="project-card">
		  <div class="project-image">🌌</div>
		  <div class="project-content">
			<h3>the-void-engine</h3>
			<p>A custom C++ game engine for experimental game development.</p>
			<div class="tech-stack">
			  <span class="tech">C++</span>
			</div>
			<div class="project-links">
			  <button class="link-btn">Live Demo</button>
			  <button onclick={() => window.open('https://github.com/erdospeet11/the-void-engine', '_blank')} class="link-btn">GitHub</button>
			</div>
		  </div>
		</div>
	  
		<div class="project-card">
		  <div class="project-image">🪐</div>
		  <div class="project-content">
			<h3>cosmosjs-multiplayer</h3>
			<p>Multiplayer JavaScript game with a cosmic theme and real-time communication.</p>
			<div class="tech-stack">
			  <span class="tech">JavaScript</span>
			</div>
			<div class="project-links">
			  <button class="link-btn">Live Demo</button>
			  <button onclick={() => window.open('https://github.com/erdospeet11/cosmosjs-multiplayer', '_blank')} class="link-btn">GitHub</button>
			</div>
		  </div>
		</div>
	  
		<div class="project-card">
		  <div class="project-image">💾</div>
		  <div class="project-content">
			<h3>dbmc</h3>
			<p>A C-based database management CLI tool for learning systems-level programming.</p>
			<div class="tech-stack">
			  <span class="tech">C</span>
			</div>
			<div class="project-links">
			  <button class="link-btn">Live Demo</button>
			  <button onclick={() => window.open('https://github.com/erdospeet11/dbmc', '_blank')} class="link-btn">GitHub</button>
			</div>
		  </div>
		</div>
	  
		<div class="project-card">
		  <div class="project-image">🏠</div>
		  <div class="project-content">
			<h3>homeinvasion</h3>
			<p>A game project built with Godot and GDScript, featuring stealth and tension mechanics.</p>
			<div class="tech-stack">
			  <span class="tech">GDScript</span>
			</div>
			<div class="project-links">
			  <button class="link-btn">Play Now</button>
			  <button onclick={() => window.open('https://github.com/erdospeet11/homeinvasion', '_blank')} class="link-btn">GitHub</button>
			</div>
		  </div>
		</div>
	  
		<div class="project-card">
		  <div class="project-image">💻</div>
		  <div class="project-content">
			<h3>computer-portfolio</h3>
			<p>Personal portfolio site built with Svelte showcasing programming projects and skills.</p>
			<div class="tech-stack">
			  <span class="tech">Svelte</span>
			</div>
			<div class="project-links">
			  <button class="link-btn">Live Demo</button>
			  <button onclick={() => window.open('https://github.com/erdospeet11/computer-portfolio', '_blank')} class="link-btn">GitHub</button>
			</div>
		  </div>
		</div>
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
		grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
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

	.project-content h3 {
		margin: 0 0 10px 0;
		color: #333;
		font-size: 18px;
	}

	.project-content p {
		margin: 0 0 15px 0;
		color: #666;
		line-height: 1.5;
		font-size: 14px;
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

	.link-btn:hover {
		background: #667eea;
		color: white;
	}

	.link-btn:first-child {
		background: #667eea;
		color: white;
	}

	.link-btn:first-child:hover {
		background: #5a6fd8;
	}
</style> 