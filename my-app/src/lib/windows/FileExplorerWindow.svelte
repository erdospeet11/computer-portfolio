<script lang="ts">
	let currentPath = 'C:\\';
	let selectedItem: string | null = null;
	let viewMode = 'details';

	const fileSystem: { [key: string]: any } = {
		'C:\\': {
			type: 'drive',
			items: [
				{ name: 'Users', type: 'folder', size: '', modified: '2024-01-15 10:30' },
				{ name: 'Program Files', type: 'folder', size: '', modified: '2024-01-10 14:20' },
				{ name: 'Program Files (x86)', type: 'folder', size: '', modified: '2024-01-10 14:20' },
				{ name: 'Windows', type: 'folder', size: '', modified: '2024-01-08 09:15' },
				{ name: 'Documents', type: 'folder', size: '', modified: '2024-01-20 16:45' },
				{ name: 'Downloads', type: 'folder', size: '', modified: '2024-01-22 11:30' },
				{ name: 'Desktop', type: 'folder', size: '', modified: '2024-01-22 12:00' },
				{ name: 'system.log', type: 'file', size: '2.4 MB', modified: '2024-01-22 09:30' },
				{ name: 'config.ini', type: 'file', size: '1.2 KB', modified: '2024-01-15 08:20' }
			]
		},
		'C:\\Users': {
			type: 'folder',
			items: [
				{ name: 'User', type: 'folder', size: '', modified: '2024-01-20 10:30' },
				{ name: 'Public', type: 'folder', size: '', modified: '2024-01-10 14:20' },
				{ name: 'Administrator', type: 'folder', size: '', modified: '2024-01-05 16:45' }
			]
		},
		'C:\\Users\\User': {
			type: 'folder',
			items: [
				{ name: 'Desktop', type: 'folder', size: '', modified: '2024-01-22 12:00' },
				{ name: 'Documents', type: 'folder', size: '', modified: '2024-01-20 16:45' },
				{ name: 'Downloads', type: 'folder', size: '', modified: '2024-01-22 11:30' },
				{ name: 'Pictures', type: 'folder', size: '', modified: '2024-01-18 14:20' },
				{ name: 'Videos', type: 'folder', size: '', modified: '2024-01-16 09:15' },
				{ name: 'Music', type: 'folder', size: '', modified: '2024-01-14 13:40' },
				{ name: 'AppData', type: 'folder', size: '', modified: '2024-01-10 08:30' }
			]
		},
		'C:\\Documents': {
			type: 'folder',
			items: [
				{ name: 'Projects', type: 'folder', size: '', modified: '2024-01-22 10:30' },
				{ name: 'Work', type: 'folder', size: '', modified: '2024-01-20 14:20' },
				{ name: 'Personal', type: 'folder', size: '', modified: '2024-01-18 16:45' },
				{ name: 'resume.pdf', type: 'file', size: '245 KB', modified: '2024-01-15 11:20' },
				{ name: 'notes.txt', type: 'file', size: '5.2 KB', modified: '2024-01-22 09:45' }
			]
		}
	};

	const drives = [
		{ letter: 'C:', name: 'Local Disk', type: 'hdd', used: '45.2 GB', total: '119 GB' },
		{ letter: 'D:', name: 'Data', type: 'hdd', used: '12.8 GB', total: '500 GB' },
		{ letter: 'E:', name: 'USB Drive', type: 'usb', used: '2.1 GB', total: '8 GB' }
	];

	$: currentItems = fileSystem[currentPath]?.items || [];
	$: pathParts = currentPath.split('\\').filter(Boolean);

	function navigateToPath(path: string) {
		if (fileSystem[path]) {
			currentPath = path;
			selectedItem = null;
		}
	}

	function navigateUp() {
		const parts = currentPath.split('\\').filter(Boolean);
		if (parts.length > 1) {
			parts.pop();
			const newPath = parts.join('\\') + '\\';
			navigateToPath(newPath);
		}
	}

	function navigateToItem(item: any) {
		if (item.type === 'folder') {
			const newPath = currentPath + (currentPath.endsWith('\\') ? '' : '\\') + item.name;
			navigateToPath(newPath);
		}
	}

	function selectItem(itemName: string) {
		selectedItem = selectedItem === itemName ? null : itemName;
	}

	function getFileIcon(type: string, name: string = '') {
		if (type === 'folder') return '📁';
		if (type === 'drive') return '💾';
		
		const ext = name.split('.').pop()?.toLowerCase();
		switch (ext) {
			case 'txt': return '📄';
			case 'pdf': return '📕';
			case 'jpg': case 'jpeg': case 'png': case 'gif': return '🖼️';
			case 'mp3': case 'wav': case 'mp4': return '🎵';
			case 'zip': case 'rar': return '📦';
			case 'exe': return '⚙️';
			case 'log': return '📋';
			case 'ini': case 'cfg': return '🔧';
			default: return '📄';
		}
	}

	function getDriveIcon(type: string) {
		switch (type) {
			case 'hdd': return '💿';
			case 'ssd': return '💾';
			case 'usb': return '🔌';
			case 'cd': return '💿';
			default: return '💾';
		}
	}

	function formatSize(size: string) {
		return size || '—';
	}
</script>

<div class="file-explorer">
	<!-- Toolbar -->
	<div class="toolbar">
		<div class="toolbar-left">
			<button class="nav-btn" on:click={navigateUp} disabled={pathParts.length <= 1} title="Up">
				⬆️
			</button>
			<button class="nav-btn" on:click={() => navigateToPath('C:\\')} title="Home">
				🏠
			</button>
		</div>
		
		<div class="address-bar">
			<span class="path-segment" on:click={() => navigateToPath('C:\\')}>C:\</span>
			{#each pathParts.slice(1) as part, i}
				<span class="path-separator">\</span>
				<span 
					class="path-segment" 
					on:click={() => navigateToPath(pathParts.slice(0, i + 2).join('\\') + '\\')}
				>
					{part}
				</span>
			{/each}
		</div>

		<div class="toolbar-right">
			<select bind:value={viewMode} class="view-selector">
				<option value="details">Details</option>
				<option value="icons">Large Icons</option>
				<option value="list">List</option>
			</select>
		</div>
	</div>

	<div class="explorer-content">
		<!-- Sidebar -->
		<div class="sidebar">
			<div class="sidebar-section">
				<h3>Quick Access</h3>
				<div class="sidebar-item" on:click={() => navigateToPath('C:\\Desktop')}>
					📁 Desktop
				</div>
				<div class="sidebar-item" on:click={() => navigateToPath('C:\\Documents')}>
					📁 Documents
				</div>
				<div class="sidebar-item" on:click={() => navigateToPath('C:\\Users\\User\\Downloads')}>
					📁 Downloads
				</div>
				<div class="sidebar-item" on:click={() => navigateToPath('C:\\Users\\User\\Pictures')}>
					📁 Pictures
				</div>
			</div>

			<div class="sidebar-section">
				<h3>This PC</h3>
				{#each drives as drive}
					<div class="sidebar-item drive-item" on:click={() => navigateToPath(drive.letter + '\\')}>
						<div class="drive-info">
							<span class="drive-icon">{getDriveIcon(drive.type)}</span>
							<div class="drive-details">
								<div class="drive-name">{drive.name} ({drive.letter})</div>
								<div class="drive-usage">{drive.used} of {drive.total}</div>
							</div>
						</div>
					</div>
				{/each}
			</div>
		</div>

		<!-- Main Content -->
		<div class="main-content">
			{#if viewMode === 'details'}
				<div class="details-view">
					<div class="details-header">
						<div class="col-name">Name</div>
						<div class="col-modified">Date Modified</div>
						<div class="col-type">Type</div>
						<div class="col-size">Size</div>
					</div>
					<div class="details-content">
						{#each currentItems as item}
							<div 
								class="details-row" 
								class:selected={selectedItem === item.name}
								on:click={() => selectItem(item.name)}
								on:dblclick={() => navigateToItem(item)}
							>
								<div class="col-name">
									<span class="file-icon">{getFileIcon(item.type, item.name)}</span>
									{item.name}
								</div>
								<div class="col-modified">{item.modified}</div>
								<div class="col-type">{item.type === 'folder' ? 'File folder' : 'File'}</div>
								<div class="col-size">{formatSize(item.size)}</div>
							</div>
						{/each}
					</div>
				</div>
			{:else if viewMode === 'icons'}
				<div class="icons-view">
					{#each currentItems as item}
						<div 
							class="icon-item"
							class:selected={selectedItem === item.name}
							on:click={() => selectItem(item.name)}
							on:dblclick={() => navigateToItem(item)}
						>
							<div class="icon-large">{getFileIcon(item.type, item.name)}</div>
							<div class="icon-name">{item.name}</div>
						</div>
					{/each}
				</div>
			{:else}
				<div class="list-view">
					{#each currentItems as item}
						<div 
							class="list-item"
							class:selected={selectedItem === item.name}
							on:click={() => selectItem(item.name)}
							on:dblclick={() => navigateToItem(item)}
						>
							<span class="file-icon">{getFileIcon(item.type, item.name)}</span>
							{item.name}
						</div>
					{/each}
				</div>
			{/if}
		</div>
	</div>

	<!-- Status Bar -->
	<div class="status-bar">
		<span>{currentItems.length} items</span>
		{#if selectedItem}
			<span>• {selectedItem} selected</span>
		{/if}
	</div>
</div>

<style>
	.file-explorer {
		height: 100%;
		display: flex;
		flex-direction: column;
		background: #fff;
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
	}

	.toolbar {
		display: flex;
		align-items: center;
		padding: 8px 12px;
		background: #f8f9fa;
		border-bottom: 1px solid #dee2e6;
		gap: 12px;
	}

	.toolbar-left {
		display: flex;
		gap: 4px;
	}

	.nav-btn {
		padding: 6px 10px;
		border: 1px solid #ccc;
		background: white;
		border-radius: 3px;
		cursor: pointer;
		font-size: 12px;
		transition: background-color 0.2s;
	}

	.nav-btn:hover:not(:disabled) {
		background: #e9ecef;
	}

	.nav-btn:disabled {
		opacity: 0.5;
		cursor: not-allowed;
	}

	.address-bar {
		flex: 1;
		display: flex;
		align-items: center;
		background: white;
		border: 1px solid #ccc;
		border-radius: 3px;
		padding: 6px 10px;
		font-size: 12px;
		overflow-x: auto;
	}

	.path-segment {
		cursor: pointer;
		padding: 2px 4px;
		border-radius: 2px;
		white-space: nowrap;
	}

	.path-segment:hover {
		background: #e9ecef;
	}

	.path-separator {
		color: #666;
		margin: 0 2px;
	}

	.view-selector {
		padding: 4px 8px;
		border: 1px solid #ccc;
		border-radius: 3px;
		font-size: 12px;
	}

	.explorer-content {
		flex: 1;
		display: flex;
		overflow: hidden;
	}

	.sidebar {
		width: 200px;
		background: #f8f9fa;
		border-right: 1px solid #dee2e6;
		overflow-y: auto;
		padding: 8px;
	}

	.sidebar-section {
		margin-bottom: 16px;
	}

	.sidebar-section h3 {
		font-size: 12px;
		font-weight: 600;
		color: #495057;
		margin: 0 0 8px 0;
		text-transform: uppercase;
		letter-spacing: 0.5px;
	}

	.sidebar-item {
		padding: 6px 8px;
		cursor: pointer;
		border-radius: 3px;
		font-size: 12px;
		margin-bottom: 2px;
		transition: background-color 0.2s;
	}

	.sidebar-item:hover {
		background: #e9ecef;
	}

	.drive-item {
		padding: 8px;
	}

	.drive-info {
		display: flex;
		align-items: center;
		gap: 8px;
	}

	.drive-icon {
		font-size: 16px;
	}

	.drive-details {
		flex: 1;
		min-width: 0;
	}

	.drive-name {
		font-size: 11px;
		font-weight: 500;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.drive-usage {
		font-size: 10px;
		color: #666;
	}

	.main-content {
		flex: 1;
		overflow: auto;
		background: white;
	}

	/* Details View */
	.details-view {
		height: 100%;
		display: flex;
		flex-direction: column;
	}

	.details-header {
		display: grid;
		grid-template-columns: 2fr 1fr 1fr 1fr;
		gap: 8px;
		padding: 8px 16px;
		background: #f8f9fa;
		border-bottom: 1px solid #dee2e6;
		font-size: 12px;
		font-weight: 600;
		color: #495057;
	}

	.details-content {
		flex: 1;
		overflow-y: auto;
	}

	.details-row {
		display: grid;
		grid-template-columns: 2fr 1fr 1fr 1fr;
		gap: 8px;
		padding: 6px 16px;
		cursor: pointer;
		font-size: 12px;
		border-bottom: 1px solid #f8f9fa;
		transition: background-color 0.1s;
	}

	.details-row:hover {
		background: #f8f9fa;
	}

	.details-row.selected {
		background: #cce7ff;
	}

	.col-name {
		display: flex;
		align-items: center;
		gap: 6px;
		overflow: hidden;
	}

	.file-icon {
		flex-shrink: 0;
		font-size: 14px;
	}

	/* Icons View */
	.icons-view {
		padding: 16px;
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
		gap: 16px;
	}

	.icon-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
		cursor: pointer;
		padding: 8px;
		border-radius: 4px;
		transition: background-color 0.1s;
	}

	.icon-item:hover {
		background: #f8f9fa;
	}

	.icon-item.selected {
		background: #cce7ff;
	}

	.icon-large {
		font-size: 32px;
		margin-bottom: 4px;
	}

	.icon-name {
		font-size: 11px;
		word-break: break-word;
		max-width: 100%;
	}

	/* List View */
	.list-view {
		padding: 8px 16px;
	}

	.list-item {
		display: flex;
		align-items: center;
		gap: 8px;
		padding: 4px 8px;
		cursor: pointer;
		font-size: 12px;
		border-radius: 3px;
		transition: background-color 0.1s;
	}

	.list-item:hover {
		background: #f8f9fa;
	}

	.list-item.selected {
		background: #cce7ff;
	}

	.status-bar {
		padding: 4px 16px;
		background: #f8f9fa;
		border-top: 1px solid #dee2e6;
		font-size: 11px;
		color: #666;
		display: flex;
		gap: 8px;
	}

	/* Scrollbar Styling */
	::-webkit-scrollbar {
		width: 12px;
		height: 12px;
	}

	::-webkit-scrollbar-track {
		background: #f1f1f1;
	}

	::-webkit-scrollbar-thumb {
		background: #c1c1c1;
		border-radius: 6px;
	}

	::-webkit-scrollbar-thumb:hover {
		background: #a8a8a8;
	}
</style> 