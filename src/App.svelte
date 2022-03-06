<script>
	import { guesses, concelhos, targetConcelho, canGuess, timeToNext } from "./stores.js";
	import Guess from './Guess.svelte';
	import FinishModal from './FinishModal.svelte';
	let notFound = false;
	let currentGuess = '';
	
	const submitGuess = (event) => {
		event.preventDefault();
		const guess = concelhos.find(concelho => concelho.concelho.toLowerCase().localeCompare(currentGuess.toLowerCase(), 'pt', {sensitivity: 'base'}) === 0);
		
		if (guess) {
			guesses.addGuess(guess);
		} else {
			console.log('Municipio nao encontrado');
			notFound = true;
		}
		currentGuess = '';
	}
</script>


{#if !$canGuess}
		<FinishModal />
	{/if}

<main>

	<img src={'svg/' + $targetConcelho.id + '.svg'} alt="Imagem do município" />
	
	<div class="guesses">
		{#each $guesses as guess}
			<Guess guess={guess}></Guess>
		{/each}
	</div>
	<form id="guess-form" on:submit={submitGuess}>
		<input type="text" id="guess-input" on:input={() => notFound = false} bind:value={currentGuess} disabled={!$canGuess}>
		<button type="submit" disabled={!$canGuess}>Submeter</button>
	</form>
	{#if notFound}
		<div class="not-found">
			Município não encontrado
		</div>
	{/if}

	
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	.guesses {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.not-found {
		color: red;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>