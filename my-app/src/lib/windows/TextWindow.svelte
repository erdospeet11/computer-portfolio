<script lang="ts">
	export let textContent: string = '';
	export let fileName: string = 'Untitled.txt';
	export let readonly: boolean = false;

	let textArea: HTMLTextAreaElement;
	let modified = false;
	let originalContent = textContent;

	function handleTextChange() {
		modified = textContent !== originalContent;
	}

	function saveFile() {
		originalContent = textContent;
		modified = false;
		
		const blob = new Blob([textContent], { type: 'text/plain' });
		const url = URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.href = url;
		a.download = fileName;
		document.body.appendChild(a);
		a.click();
		document.body.removeChild(a);
		URL.revokeObjectURL(url);
	}

	function selectAll() {
		if (textArea) {
			textArea.select();
		}
	}

	function copyText() {
		if (textArea) {
			textArea.select();
			document.execCommand('copy');
		}
	}
</script>

<div class="text-window">
	<div class="text-toolbar">
		<div class="toolbar-group">
			<button class="toolbar-btn" on:click={saveFile} title="Save File">💾 Save</button>
			{#if !readonly}
				<span class="file-status" class:modified>
					{modified ? '● Modified' : '○ Saved'}
				</span>
			{/if}
		</div>
		<div class="toolbar-group">
			<button class="toolbar-btn" on:click={selectAll} title="Select All">📋 Select All</button>
			<button class="toolbar-btn" on:click={copyText} title="Copy">📄 Copy</button>
		</div>
		<div class="file-name">{fileName}</div>
	</div>

	<div class="text-content">
		<textarea
			bind:this={textArea}
			bind:value={textContent}
			on:input={handleTextChange}
			{readonly}
			class="text-editor"
			class:readonly
			placeholder={readonly ? '' : 'Type your text here...'}
			spellcheck="false"
		></textarea>
	</div>

	{#if readonly}
		<div class="readonly-notice">
			<small>📖 This file is read-only</small>
		</div>
	{/if}
</div>

<style>
	.text-window {
		height: 100%;
		display: flex;
		flex-direction: column;
		background: #f9f9f9;
		font-family: 'Consolas', 'Monaco', 'Courier New', monospace;
	}

	.text-toolbar {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 8px 12px;
		background: #e8e8e8;
		border-bottom: 1px solid #ccc;
		flex-shrink: 0;
	}

	.toolbar-group {
		display: flex;
		align-items: center;
		gap: 8px;
	}

	.toolbar-btn {
		padding: 4px 8px;
		border: 1px solid #ccc;
		background: white;
		border-radius: 3px;
		cursor: pointer;
		font-size: 11px;
		transition: background-color 0.2s;
	}

	.toolbar-btn:hover {
		background: #f0f0f0;
	}

	.toolbar-btn:active {
		background: #e0e0e0;
	}

	.file-status {
		font-size: 11px;
		color: #666;
		font-weight: 500;
	}

	.file-status.modified {
		color: #d73a49;
	}

	.file-name {
		font-size: 12px;
		color: #666;
		font-weight: 500;
	}

	.text-content {
		flex: 1;
		display: flex;
		flex-direction: column;
	}

	.text-editor {
		flex: 1;
		border: none;
		outline: none;
		padding: 16px;
		font-family: 'Consolas', 'Monaco', 'Courier New', monospace;
		font-size: 13px;
		line-height: 1.5;
		background: white;
		color: #333;
		resize: none;
		tab-size: 4;
	}

	.text-editor.readonly {
		background: #f8f8f8;
		color: #555;
		cursor: default;
	}

	.readonly-notice {
		padding: 8px 16px;
		background: #fff3cd;
		border-top: 1px solid #ffeaa7;
		color: #856404;
		text-align: center;
	}

	.text-editor:focus {
		background: #fff;
		box-shadow: inset 0 0 0 2px rgba(0, 120, 212, 0.2);
	}

	.text-editor::placeholder {
		color: #999;
		font-style: italic;
	}
</style> 