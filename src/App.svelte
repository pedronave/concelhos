<script>
	import FinishModal from './FinishModal.svelte';
	import HelpModal from './HelpModal.svelte';
	import PlayArea from './PlayArea.svelte';

	const firstTime = localStorage.getItem('firstTime') === null;
	localStorage.setItem('firstTime', 'false');
	let showStatsModal = false;
	let showHelpModal = firstTime;
</script>

{#if showHelpModal}
	<HelpModal on:close={() => showHelpModal = false}/>
{/if}

{#if showStatsModal}
	<FinishModal on:close={() => showStatsModal = false}/>
{/if}

<nav class="navbar">
	<h1>Concelhos</h1>
	<div class="actions">
		<button on:click={() => showStatsModal = true}>
			<svg xmlns="http://www.w3.org/2000/svg" height="24" width="24" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
				<path stroke-linecap="round" stroke-linejoin="round" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z" />
			  </svg>
		</button>
		<button on:click={() => showHelpModal = true}>
			<svg xmlns="http://www.w3.org/2000/svg" height="24" width="24" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
				<path stroke-linecap="round" stroke-linejoin="round" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
			  </svg>
		</button>
	</div>
</nav>

<main>
	<PlayArea on:gameOver={() => showStatsModal = true}/>
</main>

<style>
	.navbar {
		width: 100%;
		display: flex;
		box-sizing: border-box;
		justify-content: space-between;
		align-items: center;
		max-width: 1280px;
		margin: 0 auto;
		padding: 0 1rem;
	}

	.navbar button {
		background: none;
		border: none;
		outline: none;
		cursor: pointer;
		padding: 0;
		margin: 0;
		width: 40px;
		height: 40px;
		margin: 0 0.25rem;
	}

	main {
		text-align: center;
		padding: 1em;
		max-width: 1280px;
		margin: 0 auto;
	}
</style>