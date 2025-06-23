<script lang="ts">
	let gameStarted = false;
	let cards: Array<{id: number, value: string, flipped: boolean, matched: boolean}> = [];
	let flippedCards: number[] = [];
	let score = 0;
	let moves = 0;
	let gameWon = false;
	let timeElapsed = 0;
	let gameTimer: number | null = null;

	const cardValues = ['🎮', '🎯', '🎨', '🎵', '🎲', '🃏', '🎪', '🎭'];

	function initializeGame() {
		const cardPairs = cardValues.flatMap(value => [
			{ value, flipped: false, matched: false },
			{ value, flipped: false, matched: false }
		]);
		
		cards = cardPairs
			.sort(() => Math.random() - 0.5)
			.map((card, index) => ({ ...card, id: index }));
		
		gameStarted = true;
		score = 0;
		moves = 0;
		gameWon = false;
		timeElapsed = 0;
		flippedCards = [];
		
		gameTimer = setInterval(() => {
			timeElapsed++;
		}, 1000);
	}

	function flipCard(cardId: number) {
		if (flippedCards.length >= 2 || cards[cardId].flipped || cards[cardId].matched) return;
		
		cards[cardId].flipped = true;
		flippedCards = [...flippedCards, cardId];
		
		if (flippedCards.length === 2) {
			moves++;
			setTimeout(checkMatch, 800);
		}
	}

	function checkMatch() {
		const [first, second] = flippedCards;
		
		if (cards[first].value === cards[second].value) {
			cards[first].matched = true;
			cards[second].matched = true;
			score += 10;
			
			if (cards.every(card => card.matched)) {
				gameWon = true;
				if (gameTimer) {
					clearInterval(gameTimer);
					gameTimer = null;
				}
			}
		} else {
			cards[first].flipped = false;
			cards[second].flipped = false;
		}
		
		flippedCards = [];
		cards = [...cards];
	}

	function resetGame() {
		if (gameTimer) {
			clearInterval(gameTimer);
			gameTimer = null;
		}
		gameStarted = false;
		cards = [];
		flippedCards = [];
	}

	function formatTime(seconds: number): string {
		const mins = Math.floor(seconds / 60);
		const secs = seconds % 60;
		return `${mins}:${secs.toString().padStart(2, '0')}`;
	}
</script>

<div class="card-game-window">
	{#if !gameStarted}
		<div class="game-menu">
			<div class="game-header">
				<h1>🃏 Memory Card Game</h1>
				<p>Match pairs of cards to win! Test your memory and beat your best time.</p>
			</div>
			
			<div class="game-rules">
				<h2>How to Play:</h2>
				<ul>
					<li>Click on cards to flip them over</li>
					<li>Try to find matching pairs</li>
					<li>Match all pairs to win the game</li>
					<li>Try to complete it in the fewest moves!</li>
				</ul>
			</div>
			
			<button class="start-btn" on:click={initializeGame}>
				🎮 Start New Game
			</button>
		</div>
	{:else}
		<div class="game-area">
			<div class="game-stats">
				<div class="stat">
					<span class="stat-label">Score:</span>
					<span class="stat-value">{score}</span>
				</div>
				<div class="stat">
					<span class="stat-label">Moves:</span>
					<span class="stat-value">{moves}</span>
				</div>
				<div class="stat">
					<span class="stat-label">Time:</span>
					<span class="stat-value">{formatTime(timeElapsed)}</span>
				</div>
				<button class="reset-btn" on:click={resetGame}>🔄 Reset</button>
			</div>

			{#if gameWon}
				<div class="victory-screen">
					<div class="victory-content">
						<h2>🎉 Congratulations!</h2>
						<p>You completed the game!</p>
						<div class="final-stats">
							<div>Final Score: <strong>{score}</strong></div>
							<div>Total Moves: <strong>{moves}</strong></div>
							<div>Time: <strong>{formatTime(timeElapsed)}</strong></div>
						</div>
						<button class="play-again-btn" on:click={resetGame}>
							Play Again
						</button>
					</div>
				</div>
			{/if}

			<div class="cards-grid">
				{#each cards as card (card.id)}
					<!-- svelte-ignore a11y-click-events-have-key-events -->
					<!-- svelte-ignore a11y-no-static-element-interactions -->
					<div 
						class="card" 
						class:flipped={card.flipped || card.matched}
						class:matched={card.matched}
						on:click={() => flipCard(card.id)}
					>
						<div class="card-front">?</div>
						<div class="card-back">{card.value}</div>
					</div>
				{/each}
			</div>
		</div>
	{/if}
</div>

<style>
	.card-game-window {
		padding: 20px;
		height: 100%;
		overflow-y: auto;
		font-family: 'Space Mono', 'Consolas', 'Courier New', monospace;
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		color: white;
	}

	.game-menu {
		text-align: center;
		max-width: 500px;
		margin: 0 auto;
		padding: 40px 20px;
	}

	.game-header h1 {
		margin: 0 0 15px 0;
		font-size: 32px;
		text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
	}

	.game-header p {
		margin: 0 0 30px 0;
		font-size: 16px;
		opacity: 0.9;
	}

	.game-rules {
		background: rgba(255, 255, 255, 0.1);
		padding: 20px;
		border-radius: 10px;
		margin-bottom: 30px;
		text-align: left;
	}

	.game-rules h2 {
		margin: 0 0 15px 0;
		color: white;
	}

	.game-rules ul {
		margin: 0;
		padding-left: 20px;
	}

	.game-rules li {
		margin-bottom: 8px;
		line-height: 1.4;
	}

	.start-btn {
		padding: 15px 30px;
		font-size: 18px;
		background: rgba(255, 255, 255, 0.2);
		border: 2px solid white;
		color: white;
		border-radius: 25px;
		cursor: pointer;
		transition: all 0.3s;
		font-weight: 600;
	}

	.start-btn:hover {
		background: white;
		color: #667eea;
		transform: translateY(-2px);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
	}

	.game-area {
		position: relative;
	}

	.game-stats {
		display: flex;
		justify-content: space-between;
		align-items: center;
		background: rgba(255, 255, 255, 0.1);
		padding: 15px 20px;
		border-radius: 10px;
		margin-bottom: 20px;
		flex-wrap: wrap;
		gap: 15px;
	}

	.stat {
		display: flex;
		align-items: center;
		gap: 8px;
	}

	.stat-label {
		font-weight: 500;
		opacity: 0.8;
	}

	.stat-value {
		font-weight: 700;
		font-size: 18px;
	}

	.reset-btn {
		padding: 8px 16px;
		background: rgba(255, 255, 255, 0.2);
		border: 1px solid rgba(255, 255, 255, 0.5);
		color: white;
		border-radius: 5px;
		cursor: pointer;
		font-size: 14px;
		transition: background-color 0.2s;
	}

	.reset-btn:hover {
		background: rgba(255, 255, 255, 0.3);
	}

	.cards-grid {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 15px;
		max-width: 400px;
		margin: 0 auto;
	}

	.card {
		aspect-ratio: 1;
		position: relative;
		cursor: pointer;
		transform-style: preserve-3d;
		transition: transform 0.6s;
	}

	.card.flipped {
		transform: rotateY(180deg);
	}

	.card.matched {
		opacity: 0.7;
		cursor: not-allowed;
	}

	.card-front,
	.card-back {
		position: absolute;
		width: 100%;
		height: 100%;
		backface-visibility: hidden;
		border-radius: 8px;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 24px;
		font-weight: bold;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
	}

	.card-front {
		background: linear-gradient(135deg, #ff6b6b, #ee5a24);
		color: white;
		font-size: 32px;
	}

	.card-back {
		background: white;
		color: #333;
		transform: rotateY(180deg);
		font-size: 28px;
	}

	.victory-screen {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: rgba(0, 0, 0, 0.8);
		display: flex;
		align-items: center;
		justify-content: center;
		z-index: 1000;
	}

	.victory-content {
		background: white;
		color: #333;
		padding: 40px;
		border-radius: 15px;
		text-align: center;
		box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
	}

	.victory-content h2 {
		margin: 0 0 15px 0;
		color: #667eea;
	}

	.victory-content p {
		margin: 0 0 20px 0;
		color: #666;
	}

	.final-stats {
		margin: 20px 0;
		padding: 15px;
		background: #f8f9fa;
		border-radius: 8px;
	}

	.final-stats div {
		margin-bottom: 5px;
		color: #555;
	}

	.play-again-btn {
		padding: 12px 24px;
		background: #667eea;
		color: white;
		border: none;
		border-radius: 6px;
		cursor: pointer;
		font-size: 16px;
		font-weight: 600;
		transition: background-color 0.2s;
	}

	.play-again-btn:hover {
		background: #5a6fd8;
	}

	@media (max-width: 480px) {
		.cards-grid {
			max-width: 300px;
			gap: 10px;
		}
		
		.card-front, .card-back {
			font-size: 20px;
		}
	}
</style> 