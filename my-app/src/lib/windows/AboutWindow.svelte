<div class="about-window">
	<div class="profile-section">
		<div class="avatar">👤</div>
		<div class="info">
			<h1>Erdős Péter Zsombor</h1>
			<p class="title">Student</p>
			<p class="location">📍 Budapest, Hungary</p>
		</div>
	</div>
	
	<div class="content">
		<section>
			<h2>About Me</h2>
			<p>
				Welcome to my digital workspace! I'm a student at the University of Eötvös Loránd, Faculty of Informatics.
				I'm interested in software development and game development.
			</p>
		</section>

		<section>
			<h2>Skills</h2>
			<div class="skills-grid">
				<div class="skill-item">JavaScript</div>
				<div class="skill-item">TypeScript</div>
				<div class="skill-item">React</div>
				<div class="skill-item">Svelte</div>
				<div class="skill-item">Node.js</div>
				<div class="skill-item">Python</div>
				<div class="skill-item">C#</div>
				<div class="skill-item">ASP .NET</div>
				<div class="skill-item">Git</div>
			</div>
		</section>

		<section>
			<h2>Experience</h2>
			<div class="experience-item">
			  <h3>Digital Solutions Intern</h3>
			  <p class="company">Ernst & Young Advisory Ltd. • December 2024 - May 2025 • Budapest</p>
			  <p>I developed RAG-based chatbot applications for various corporate departments using .NET and React, along with Azure technologies. I also created other internal tools using ExcelScript, Power Platform, and Python to automate business processes and improve efficiency.</p>
			</div>
			<div class="experience-item">
			  <h3>Student Job</h3>
			  <p class="company">Kofax-Recognita • June 2022 - August 2022 • Budapest</p>
			  <p>I tested an optical character recognition (OCR) software at the company. My tasks included collecting scanned texts in foreign languages, uploading them into the software, and verifying whether the software correctly recognized the content (primarily in Hebrew).</p>
			</div>
		  </section>
	</div>
</div>

<style>
	.about-window {
		padding: 20px;
		height: 100%;
		overflow-y: auto;
		font-family: 'Space Mono', 'Consolas', 'Courier New', monospace;
	}

	.profile-section {
		display: flex;
		align-items: center;
		gap: 20px;
		margin-bottom: 30px;
		padding-bottom: 20px;
		border-bottom: 1px solid #eee;
	}

	.avatar {
		font-size: 80px;
		width: 120px;
		height: 120px;
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		color: white;
	}

	.info h1 {
		margin: 0 0 5px 0;
		color: #333;
		font-size: 28px;
	}

	.title {
		font-size: 18px;
		color: #666;
		margin: 0 0 10px 0;
		font-weight: 500;
	}

	.location {
		margin: 0;
		color: #888;
		font-size: 14px;
	}

	.content section {
		margin-bottom: 25px;
	}

	.content h2 {
		color: #333;
		border-bottom: 2px solid #667eea;
		padding-bottom: 5px;
		margin-bottom: 15px;
	}

	.content p {
		line-height: 1.6;
		color: #555;
	}

	.skills-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
		gap: 10px;
		margin-top: 15px;
	}

	.skill-item {
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		color: white;
		padding: 8px 12px;
		border-radius: 20px;
		text-align: center;
		font-size: 14px;
		font-weight: 500;
	}

	.experience-item {
		margin-bottom: 20px;
		padding: 15px;
		border-left: 3px solid #667eea;
		background: #f8f9fa;
		border-radius: 0 5px 5px 0;
	}

	.experience-item h3 {
		margin: 0 0 5px 0;
		color: #333;
	}

	.company {
		margin: 0 0 10px 0;
		color: #667eea;
		font-weight: 500;
		font-size: 14px;
	}

	.experience-item p:last-child {
		margin: 0;
		color: #666;
	}
</style> 