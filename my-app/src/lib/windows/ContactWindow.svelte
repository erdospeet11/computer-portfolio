<script lang="ts">
	let name = '';
	let email = '';
	let message = '';
	let isSubmitting = false;
	let isSubmitted = false;

	async function handleSubmit() {
		isSubmitting = true;
		// Simulate form submission
		await new Promise(resolve => setTimeout(resolve, 1000));
		isSubmitted = true;
		isSubmitting = false;
		
		// Reset form after 3 seconds
		setTimeout(() => {
			name = '';
			email = '';
			message = '';
			isSubmitted = false;
		}, 3000);
	}
</script>

<div class="contact-window">
	<div class="header">
		<h1>Get In Touch</h1>
		<p>Let's discuss your next project or collaboration opportunity</p>
	</div>

	<div class="contact-content">
		<div class="contact-info">
			<h2>Contact Information</h2>
			<div class="info-item">
				<span class="icon">📧</span>
				<div>
					<strong>Email</strong>
					<p>john.doe@example.com</p>
				</div>
			</div>
			<div class="info-item">
				<span class="icon">📱</span>
				<div>
					<strong>Phone</strong>
					<p>+1 (555) 123-4567</p>
				</div>
			</div>
			<div class="info-item">
				<span class="icon">📍</span>
				<div>
					<strong>Location</strong>
					<p>San Francisco, CA</p>
				</div>
			</div>

			<div class="social-links">
				<h3>Follow Me</h3>
				<div class="social-grid">
					<button class="social-btn linkedin">LinkedIn</button>
					<button class="social-btn github">GitHub</button>
					<button class="social-btn twitter">Twitter</button>
					<button class="social-btn dribbble">Dribbble</button>
				</div>
			</div>
		</div>

		<div class="contact-form">
			{#if isSubmitted}
				<div class="success-message">
					<div class="success-icon">✅</div>
					<h3>Message Sent!</h3>
					<p>Thank you for reaching out. I'll get back to you soon!</p>
				</div>
			{:else}
				<form on:submit|preventDefault={handleSubmit}>
					<div class="form-group">
						<label for="name">Name</label>
						<input 
							type="text" 
							id="name" 
							bind:value={name} 
							required 
							disabled={isSubmitting}
						>
					</div>
					<div class="form-group">
						<label for="email">Email</label>
						<input 
							type="email" 
							id="email" 
							bind:value={email} 
							required 
							disabled={isSubmitting}
						>
					</div>
					<div class="form-group">
						<label for="message">Message</label>
						<textarea 
							id="message" 
							bind:value={message} 
							rows="5" 
							required 
							disabled={isSubmitting}
						></textarea>
					</div>
					<button type="submit" class="submit-btn" disabled={isSubmitting}>
						{isSubmitting ? 'Sending...' : 'Send Message'}
					</button>
				</form>
			{/if}
		</div>
	</div>
</div>

<style>
	.contact-window {
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

	.contact-content {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 30px;
	}

	@media (max-width: 768px) {
		.contact-content {
			grid-template-columns: 1fr;
		}
	}

	.contact-info h2 {
		color: #333;
		margin-bottom: 20px;
		border-bottom: 2px solid #667eea;
		padding-bottom: 5px;
	}

	.info-item {
		display: flex;
		align-items: flex-start;
		gap: 15px;
		margin-bottom: 20px;
	}

	.icon {
		font-size: 24px;
		margin-top: 2px;
	}

	.info-item strong {
		display: block;
		color: #333;
		margin-bottom: 5px;
	}

	.info-item p {
		margin: 0;
		color: #666;
	}

	.social-links {
		margin-top: 30px;
	}

	.social-links h3 {
		color: #333;
		margin-bottom: 15px;
	}

	.social-grid {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 10px;
	}

	.social-btn {
		padding: 10px;
		border: none;
		border-radius: 6px;
		cursor: pointer;
		font-weight: 500;
		transition: transform 0.2s;
	}

	.social-btn:hover {
		transform: translateY(-1px);
	}

	.linkedin { background: #0077b5; color: white; }
	.github { background: #333; color: white; }
	.twitter { background: #1da1f2; color: white; }
	.dribbble { background: #ea4c89; color: white; }

	.contact-form {
		background: #f8f9fa;
		padding: 25px;
		border-radius: 8px;
		border: 1px solid #e9ecef;
	}

	.form-group {
		margin-bottom: 20px;
	}

	.form-group label {
		display: block;
		margin-bottom: 8px;
		color: #333;
		font-weight: 500;
	}

	.form-group input,
	.form-group textarea {
		width: 100%;
		padding: 10px 12px;
		border: 1px solid #ddd;
		border-radius: 4px;
		font-size: 14px;
		font-family: inherit;
		transition: border-color 0.2s;
	}

	.form-group input:focus,
	.form-group textarea:focus {
		outline: none;
		border-color: #667eea;
	}

	.form-group textarea {
		resize: vertical;
		min-height: 100px;
	}

	.submit-btn {
		width: 100%;
		padding: 12px;
		background: #667eea;
		color: white;
		border: none;
		border-radius: 4px;
		font-size: 16px;
		font-weight: 500;
		cursor: pointer;
		transition: background-color 0.2s;
	}

	.submit-btn:hover:not(:disabled) {
		background: #5a6fd8;
	}

	.submit-btn:disabled {
		opacity: 0.6;
		cursor: not-allowed;
	}

	.success-message {
		text-align: center;
		padding: 40px 20px;
	}

	.success-icon {
		font-size: 48px;
		margin-bottom: 15px;
	}

	.success-message h3 {
		color: #28a745;
		margin-bottom: 10px;
	}

	.success-message p {
		color: #666;
		margin: 0;
	}
</style> 