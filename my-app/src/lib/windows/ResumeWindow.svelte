<script lang="ts">
	import { browser } from '$app/environment';

	let resumeContent: HTMLElement;

	async function downloadPDF() {
		if (!browser) return;
		
		try {
			// @ts-ignore - html2pdf.js doesn't have official types and is dynamically imported
			const html2pdf = await import('html2pdf.js');
			const element = resumeContent;
			const opt = {
				margin: 1,
				filename: 'John_Doe_Resume.pdf',
				image: { type: 'jpeg', quality: 0.98 },
				html2canvas: { scale: 2, useCORS: true },
				jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
			};

			html2pdf.default().set(opt).from(element).save();
		} catch (error) {
			console.error('Error generating PDF:', error);
			alert('Sorry, there was an error generating the PDF. Please try the print option instead.');
		}
	}

	function printResume() {
		window.print();
	}
</script>

<div class="resume-window">
	<div class="resume-header">
		<div class="header-actions">
			<button class="action-btn" on:click={downloadPDF}>📄 Download PDF</button>
			<button class="action-btn" on:click={printResume}>🖨️ Print</button>
		</div>
	</div>

	<div class="resume-content" bind:this={resumeContent}>
		<header class="resume-header-section">
			<h1>John Doe</h1>
			<p class="title">Full Stack Developer</p>
			<div class="contact-info">
				<span>john.doe@example.com</span>
				<span>•</span>
				<span>+1 (555) 123-4567</span>
				<span>•</span>
				<span>San Francisco, CA</span>
				<span>•</span>
				<span>linkedin.com/in/johndoe</span>
			</div>
		</header>

		<section class="resume-section">
			<h2>Professional Summary</h2>
			<p>
				Experienced Full Stack Developer with 5+ years of expertise in building scalable web applications 
				and leading development teams. Proficient in modern JavaScript frameworks, cloud technologies, 
				and agile methodologies. Passionate about creating efficient solutions and mentoring junior developers.
			</p>
		</section>

		<section class="resume-section">
			<h2>Technical Skills</h2>
			<div class="skills-section">
				<div class="skill-category">
					<h3>Frontend</h3>
					<p>React, Vue.js, Svelte, TypeScript, HTML5, CSS3, Sass, Tailwind CSS</p>
				</div>
				<div class="skill-category">
					<h3>Backend</h3>
					<p>Node.js, Express, Python, Django, PHP, RESTful APIs, GraphQL</p>
				</div>
				<div class="skill-category">
					<h3>Database</h3>
					<p>PostgreSQL, MongoDB, MySQL, Redis, Elasticsearch</p>
				</div>
				<div class="skill-category">
					<h3>Tools & Technologies</h3>
					<p>Git, Docker, AWS, CI/CD, Jest, Webpack, Linux</p>
				</div>
			</div>
		</section>

		<section class="resume-section">
			<h2>Professional Experience</h2>
			
			<div class="experience-item">
				<div class="experience-header">
					<div>
						<h3>Senior Full Stack Developer</h3>
						<p class="company">Tech Solutions Inc.</p>
					</div>
					<div class="duration">2022 - Present</div>
				</div>
				<ul>
					<li>Led development of microservices architecture serving 100k+ daily active users</li>
					<li>Mentored team of 4 junior developers and conducted code reviews</li>
					<li>Implemented CI/CD pipelines reducing deployment time by 60%</li>
					<li>Collaborated with product team to define technical requirements</li>
				</ul>
			</div>

			<div class="experience-item">
				<div class="experience-header">
					<div>
						<h3>Full Stack Developer</h3>
						<p class="company">Startup Ventures</p>
					</div>
					<div class="duration">2020 - 2022</div>
				</div>
				<ul>
					<li>Built responsive web applications using React and Node.js</li>
					<li>Designed and implemented RESTful APIs and database schemas</li>
					<li>Optimized application performance improving load times by 40%</li>
					<li>Participated in agile development process and sprint planning</li>
				</ul>
			</div>

			<div class="experience-item">
				<div class="experience-header">
					<div>
						<h3>Frontend Developer</h3>
						<p class="company">Digital Agency Co.</p>
					</div>
					<div class="duration">2019 - 2020</div>
				</div>
				<ul>
					<li>Developed responsive websites for various clients using modern frameworks</li>
					<li>Collaborated with designers to implement pixel-perfect UI components</li>
					<li>Maintained and updated existing web applications</li>
				</ul>
			</div>
		</section>

		<section class="resume-section">
			<h2>Education</h2>
			<div class="education-item">
				<div class="education-header">
					<div>
						<h3>Bachelor of Science in Computer Science</h3>
						<p class="school">University of California, San Francisco</p>
					</div>
					<div class="duration">2015 - 2019</div>
				</div>
				<p>Graduated Magna Cum Laude • GPA: 3.8/4.0</p>
			</div>
		</section>

		<section class="resume-section">
			<h2>Certifications</h2>
			<ul class="certifications-list">
				<li>AWS Certified Solutions Architect (2023)</li>
				<li>Google Cloud Professional Developer (2022)</li>
				<li>Certified Scrum Master (2021)</li>
			</ul>
		</section>
	</div>
</div>

<style>
	.resume-window {
		height: 100%;
		overflow-y: auto;
		font-family: 'Space Mono', 'Consolas', 'Courier New', monospace;
		background: white;
	}

	.resume-header {
		padding: 15px 20px;
		border-bottom: 1px solid #eee;
		background: #f8f9fa;
	}

	.header-actions {
		display: flex;
		gap: 10px;
	}

	.action-btn {
		padding: 8px 16px;
		border: 1px solid #ddd;
		background: white;
		border-radius: 4px;
		cursor: pointer;
		font-size: 12px;
		transition: background-color 0.2s;
	}

	.action-btn:hover {
		background: #f0f0f0;
	}

	.resume-content {
		padding: 30px;
		max-width: 800px;
		margin: 0 auto;
	}

	.resume-header-section {
		text-align: center;
		margin-bottom: 30px;
		padding-bottom: 20px;
		border-bottom: 2px solid #333;
	}

	.resume-header-section h1 {
		margin: 0 0 5px 0;
		font-size: 32px;
		color: #333;
		font-weight: 700;
	}

	.title {
		margin: 0 0 15px 0;
		font-size: 18px;
		color: #666;
		font-weight: 500;
	}

	.contact-info {
		font-size: 14px;
		color: #666;
		display: flex;
		justify-content: center;
		flex-wrap: wrap;
		gap: 8px;
	}

	.resume-section {
		margin-bottom: 25px;
	}

	.resume-section h2 {
		margin: 0 0 15px 0;
		font-size: 18px;
		color: #333;
		font-weight: 600;
		text-transform: uppercase;
		letter-spacing: 1px;
		border-bottom: 1px solid #333;
		padding-bottom: 5px;
	}

	.resume-section p {
		margin: 0 0 10px 0;
		line-height: 1.6;
		color: #555;
	}

	.skills-section {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		gap: 15px;
	}

	.skill-category h3 {
		margin: 0 0 8px 0;
		color: #333;
		font-size: 14px;
		font-weight: 600;
	}

	.skill-category p {
		margin: 0;
		font-size: 13px;
		color: #666;
		line-height: 1.4;
	}

	.experience-item {
		margin-bottom: 20px;
	}

	.experience-header {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		margin-bottom: 10px;
	}

	.experience-item h3 {
		margin: 0 0 3px 0;
		color: #333;
		font-size: 16px;
		font-weight: 600;
	}

	.company, .school {
		margin: 0;
		color: #666;
		font-size: 14px;
		font-style: italic;
	}

	.duration {
		color: #666;
		font-size: 14px;
		font-weight: 500;
		white-space: nowrap;
	}

	.experience-item ul {
		margin: 0;
		padding-left: 20px;
	}

	.experience-item li {
		margin-bottom: 5px;
		line-height: 1.5;
		color: #555;
		font-size: 14px;
	}

	.education-item {
		margin-bottom: 15px;
	}

	.education-header {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		margin-bottom: 5px;
	}

	.education-item h3 {
		margin: 0 0 3px 0;
		color: #333;
		font-size: 16px;
		font-weight: 600;
	}

	.certifications-list {
		margin: 0;
		padding-left: 20px;
	}

	.certifications-list li {
		margin-bottom: 5px;
		color: #555;
		font-size: 14px;
	}

	@media print {
		.resume-header {
			display: none;
		}
		
		.resume-content {
			padding: 0;
		}
	}
</style> 