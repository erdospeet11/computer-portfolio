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
				filename: 'Erdos_Peter_Zsombor_Resume.pdf',
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
			<h1>Erdős Péter Zsombor</h1>
			<p class="title">Computer Science Student & Developer</p>
			<div class="contact-info">
				<span>erdospeter@gmail.com</span>
				<span>•</span>
				<span>+36 30 576 7616</span>
				<span>•</span>
				<span>Budapest, Hungary</span>
			</div>
		</header>

		<section class="resume-section">
			<h2>Motivation & Personality</h2>
			<p>
				Enthusiastic computer science student who primarily wants to develop their professional knowledge 
				in various areas of programming. I take pride in being able to quickly learn new technologies 
				and work efficiently in a team. In my free time, I engage in game development, which increasingly 
				interests me, but I am open to other opportunities as well.
			</p>
		</section>

		<section class="resume-section">
			<h2>Education</h2>
			<div class="education-item">
				<div class="education-header">
					<div>
						<h3>BSc Computer Science</h3>
						<p class="school">Eötvös Loránd University, Budapest</p>
					</div>
					<div class="duration">2021 - 2026</div>
				</div>
				<p>Focus on programming, software development, and computer science fundamentals</p>
			</div>
		</section>

		<section class="resume-section">
			<h2>Professional Experience</h2>
			
			<div class="experience-item">
				<div class="experience-header">
					<div>
						<h3>Digital Solutions Intern</h3>
						<p class="company">Ernst & Young Advisory Kft., Budapest</p>
					</div>
					<div class="duration">Dec 2024 - May 2025</div>
				</div>
				<ul>
					<li>Developed RAG-based chatbot applications for various corporate departments using .NET and React</li>
					<li>Worked with Azure technologies to implement cloud-based solutions</li>
					<li>Created internal tools using ExcelScript, Power Platform and Python</li>
					<li>Focused on automating business processes and improving efficiency</li>
				</ul>
			</div>

			<div class="experience-item">
				<div class="experience-header">
					<div>
						<h3>Student Worker</h3>
						<p class="company">Kofax-Recognition, Budapest</p>
					</div>
					<div class="duration">Jun 2022 - Aug 2022</div>
				</div>
				<ul>
					<li>Tested optical character recognition (OCR) software</li>
					<li>Collected scanned texts in foreign languages for testing purposes</li>
					<li>Uploaded documents to software and verified text recognition accuracy</li>
					<li>Primarily worked with Hebrew language text recognition</li>
				</ul>
			</div>
		</section>

		<section class="resume-section">
			<h2>Projects</h2>
			
			<div class="experience-item">
				<div class="experience-header">
					<div>
						<h3>Multiplayer Bomberman Game</h3>
						<p class="company">University Course Project</p>
					</div>
				</div>
				<ul>
					<li>Developed local multiplayer game with 2 teammates as part of university coursework</li>
					<li>Built using Go programming language and Ebiten framework</li>
					<li>Implemented game mechanics, multiplayer functionality, and user interface</li>
				</ul>
			</div>

			<div class="experience-item">
				<div class="experience-header">
					<div>
						<h3>Arena Survival Web Game</h3>
						<p class="company">Bachelor's Thesis Project</p>
					</div>
				</div>
				<ul>
					<li>Created web-based arena survival game for bachelor's thesis</li>
					<li>Developed using TypeScript and Phaser 3 framework</li>
					<li>Integrated MySQL database for player data storage</li>
					<li>Implemented user authentication and game mechanics</li>
				</ul>
			</div>
		</section>

		<section class="resume-section">
			<h2>Technical Skills</h2>
			<div class="skills-section">
				<div class="skill-category">
					<h3>Programming Languages</h3>
					<p>Go, Python, C#/ASP.NET, JavaScript/TypeScript, GDScript</p>
				</div>
				<div class="skill-category">
					<h3>Technologies & Frameworks</h3>
					<p>React, Azure Cloud Services, Git, HTML/CSS, Microsoft Power Platform, Phaser 3, Ebiten</p>
				</div>
				<div class="skill-category">
					<h3>Methodologies & Skills</h3>
					<p>Agile Development, Team Collaboration, Test-Driven Development, Documentation</p>
				</div>
				<div class="skill-category">
					<h3>Languages & Interests</h3>
					<p>English (B2), Game Development, 3D Modeling, Rock Climbing</p>
				</div>
			</div>
		</section>

		<section class="resume-section">
			<h2>Additional Information</h2>
			<div class="education-item">
				<p><strong>Location:</strong> Gödöllői utca 165. III/7., 1141 Budapest, Hungary</p>
				<p><strong>Availability:</strong> Open to internships, part-time and full-time opportunities</p>
				<p><strong>Special Interests:</strong> Game development, web applications, cloud technologies</p>
			</div>
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

	@media print {
		.resume-header {
			display: none;
		}
		
		.resume-content {
			padding: 0;
		}
	}
</style> 