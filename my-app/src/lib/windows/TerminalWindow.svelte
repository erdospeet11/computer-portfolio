<script lang="ts">
	import { onMount, tick } from 'svelte';

	interface HistoryEntry {
		type: 'command' | 'output' | 'error';
		content: string;
	}

	let currentInput = '';
	let history: HistoryEntry[] = [
		{ type: 'output', content: 'Windows Command Prompt [Version 1.0.0]' },
		{ type: 'output', content: '(c) Portfolio Desktop. All rights reserved.' },
		{ type: 'output', content: '' },
		{ type: 'output', content: 'Type "help" for available commands.' }
	];
	let commandHistory: string[] = [];
	let historyIndex = -1;
	let terminal: HTMLDivElement;
	let input: HTMLInputElement;

	const currentDirectory = 'C:\\Users\\Portfolio>';
	const fileSystem = {
		'about.txt': 'John Doe - Full Stack Developer\nExperienced in React, Svelte, Node.js, and more!\nContact: john.doe@example.com',
		'projects.md': '# My Projects\n\n1. E-Commerce Platform\n2. Task Management App\n3. Analytics Dashboard\n4. Card Game Suite',
		'resume.pdf': '[Binary file - Resume document]',
		'skills.txt': 'JavaScript, TypeScript, React, Svelte, Node.js, Python, PostgreSQL, MongoDB'
	};

	onMount(() => {
		input?.focus();
	});

	async function executeCommand(command: string) {
		const trimmedCommand = command.trim().toLowerCase();
		const args = trimmedCommand.split(' ');
		const cmd = args[0];

		history = [...history, { type: 'command', content: `${currentDirectory} ${command}` }];
		
		if (command.trim()) {
			commandHistory = [command, ...commandHistory.slice(0, 49)];
		}
		historyIndex = -1;

		let output = '';

		switch (cmd) {
			case 'help':
				output = `Available commands:
				
dir         - List files in current directory
ls          - Same as dir (Unix style)
type        - Display file contents (e.g., type about.txt)
cat         - Same as type (Unix style)
clear       - Clear the terminal screen
date        - Show current date and time
echo        - Display a message (e.g., echo Hello World)
whoami      - Display current user
ver         - Show system version
tree        - Show directory structure
calc        - Simple calculator (e.g., calc 2+2)
joke        - Tell a random programming joke
about       - Show information about this terminal
exit        - Close the terminal window
help        - Show this help message`;
				break;

			case 'dir':
			case 'ls':
				output = `Directory of C:\\Users\\Portfolio
				
${new Date().toLocaleDateString()} ${new Date().toLocaleTimeString()}    <DIR>          .
${new Date().toLocaleDateString()} ${new Date().toLocaleTimeString()}    <DIR>          ..
${new Date().toLocaleDateString()} ${new Date().toLocaleTimeString()}           254 about.txt
${new Date().toLocaleDateString()} ${new Date().toLocaleTimeString()}           128 projects.md
${new Date().toLocaleDateString()} ${new Date().toLocaleTimeString()}        15,250 resume.pdf
${new Date().toLocaleDateString()} ${new Date().toLocaleTimeString()}           89 skills.txt
               4 File(s)     15,721 bytes
               2 Dir(s)   unlimited bytes free`;
				break;

			case 'type':
			case 'cat':
				const filename = args[1];
				if (!filename) {
					output = 'Error: Please specify a filename.';
					history = [...history, { type: 'error', content: output }];
				} else if (fileSystem[filename as keyof typeof fileSystem]) {
					output = fileSystem[filename as keyof typeof fileSystem];
				} else {
					output = `Error: File '${filename}' not found.`;
					history = [...history, { type: 'error', content: output }];
				}
				break;

			case 'clear':
				history = [];
				currentInput = '';
				await tick();
				return;

			case 'date':
				output = new Date().toString();
				break;

			case 'echo':
				output = args.slice(1).join(' ') || '';
				break;

			case 'whoami':
				output = 'portfolio\\developer';
				break;

			case 'ver':
				output = 'Portfolio Desktop OS [Version 1.0.0]';
				break;

			case 'tree':
				output = `C:\\Users\\Portfolio
├── about.txt
├── projects.md
├── resume.pdf
└── skills.txt`;
				break;

			case 'calc':
				const expression = args.slice(1).join('');
				try {
					const sanitized = expression.replace(/[^0-9+\-*/().]/g, '');
					if (sanitized !== expression) {
						output = 'Error: Only numbers and basic operators (+, -, *, /, parentheses) are allowed.';
						history = [...history, { type: 'error', content: output }];
					} else {
						const result = Function('"use strict"; return (' + sanitized + ')')();
						output = `${expression} = ${result}`;
					}
				} catch (error) {
					output = 'Error: Invalid expression.';
					history = [...history, { type: 'error', content: output }];
				}
				break;

			case 'joke':
				const jokes = [
					"Why do programmers prefer dark mode? Because light attracts bugs!",
					"How many programmers does it take to change a light bulb? None. That's a hardware problem.",
					"Why don't programmers like nature? It has too many bugs.",
					"What's a programmer's favorite hangout place? Foo Bar!",
					"Why did the programmer quit his job? He didn't get arrays.",
					"How do you comfort a JavaScript bug? You console it!",
					"Why do Java developers wear glasses? Because they can't C#!"
				];
				output = jokes[Math.floor(Math.random() * jokes.length)];
				break;

			case 'about':
				output = `Portfolio Desktop Terminal v1.0.0
Built with Svelte and TypeScript
A simulated command line interface for portfolio demonstration
Created by: Portfolio Developer

This terminal supports basic file operations, calculations, and utility commands.
Type 'help' to see all available commands.`;
				break;

			case 'exit':
				output = 'Terminal session ended. You can close this window.';
				break;

			case '':
				break;

			default:
				output = `'${cmd}' is not recognized as an internal or external command, operable program or batch file.`;
				history = [...history, { type: 'error', content: output }];
		}

		if (output && cmd !== '') {
			history = [...history, { type: 'output', content: output }];
		}

		currentInput = '';
		await tick();
		scrollToBottom();
	}

	function handleKeyDown(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			executeCommand(currentInput);
		} else if (event.key === 'ArrowUp') {
			event.preventDefault();
			if (historyIndex < commandHistory.length - 1) {
				historyIndex++;
				currentInput = commandHistory[historyIndex];
			}
		} else if (event.key === 'ArrowDown') {
			event.preventDefault();
			if (historyIndex > 0) {
				historyIndex--;
				currentInput = commandHistory[historyIndex];
			} else if (historyIndex === 0) {
				historyIndex = -1;
				currentInput = '';
			}
		} else if (event.ctrlKey && event.key === 'l') {
			event.preventDefault();
			executeCommand('clear');
		} else if (event.ctrlKey && event.key === 'c') {
			event.preventDefault();
			history = [...history, { type: 'command', content: `${currentDirectory} ${currentInput}^C` }];
			currentInput = '';
		}
	}

	function scrollToBottom() {
		if (terminal) {
			terminal.scrollTop = terminal.scrollHeight;
		}
	}

	function focusInput() {
		input?.focus();
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="terminal-window" on:click={focusInput}>
	<div class="terminal-content" bind:this={terminal}>
		{#each history as entry}
			<div class="terminal-line {entry.type}">
				{#if entry.type === 'command'}
					<span class="prompt-text">{entry.content}</span>
				{:else}
					<pre class="output-text">{entry.content}</pre>
				{/if}
			</div>
		{/each}
		
		<div class="terminal-line current">
			<span class="prompt">{currentDirectory}</span>
			<input
				bind:this={input}
				bind:value={currentInput}
				on:keydown={handleKeyDown}
				class="terminal-input"
				autocomplete="off"
				spellcheck="false"
			/>
			<span class="cursor">_</span>
		</div>
	</div>
</div>

<style>
	.terminal-window {
		height: 100%;
		background: #000000;
		color: #00ff00;
		font-family: 'Consolas', 'Courier New', monospace;
		font-size: 14px;
		line-height: 1.4;
		overflow: hidden;
		display: flex;
		flex-direction: column;
	}

	.terminal-header {
		background: #1e1e1e;
		padding: 8px 12px;
		border-bottom: 1px solid #333;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.header-info {
		display: flex;
		flex-direction: column;
		gap: 2px;
	}

	.terminal-title {
		color: #ffffff;
		font-weight: 600;
		font-size: 12px;
	}

	.terminal-path {
		color: #888888;
		font-size: 11px;
	}

	.terminal-content {
		flex: 1;
		padding: 12px;
		overflow-y: auto;
		overflow-x: hidden;
	}

	.terminal-line {
		margin-bottom: 2px;
		word-break: break-all;
	}

	.terminal-line.command .prompt-text {
		color: #00ff00;
	}

	.terminal-line.output .output-text {
		color: #00ff00;
		margin: 0;
		white-space: pre-wrap;
		font-family: inherit;
	}

	.terminal-line.error .output-text {
		color: #ff4444;
	}

	.terminal-line.current {
		display: flex;
		align-items: center;
	}

	.prompt {
		color: #00ff00;
		white-space: nowrap;
		margin-right: 4px;
	}

	.terminal-input {
		background: transparent;
		border: none;
		color: #00ff00;
		font-family: inherit;
		font-size: inherit;
		outline: none;
		flex: 1;
		min-width: 0;
	}

	.cursor {
		color: #00ff00;
		animation: blink 1s infinite;
		margin-left: 2px;
	}

	@keyframes blink {
		0%, 50% { opacity: 1; }
		51%, 100% { opacity: 0; }
	}

	.terminal-content::-webkit-scrollbar {
		width: 8px;
	}

	.terminal-content::-webkit-scrollbar-track {
		background: #1e1e1e;
	}

	.terminal-content::-webkit-scrollbar-thumb {
		background: #555;
		border-radius: 4px;
	}

	.terminal-content::-webkit-scrollbar-thumb:hover {
		background: #777;
	}
</style> 