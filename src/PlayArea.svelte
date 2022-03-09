<script>
	import { guesses, concelhos, targetConcelho, canGuess } from "./stores.js";
	import { createEventDispatcher } from 'svelte';
	import Guess from './Guess.svelte';
	
	let notFound = false;
	let currentGuess = '';

	const dispatch = createEventDispatcher();
	
	const submitGuess = (event) => {
		event.preventDefault();
		const cleanedUpValue = currentGuess.toLowerCase().trim().replace(/\s+/g, ' ');
		const guess = concelhos.find(concelho => concelho.concelho.toLowerCase().localeCompare(cleanedUpValue, 'pt', {sensitivity: 'base'}) === 0);
		
		if (guess) {
			guesses.addGuess(guess);
			currentGuess = '';
			canGuess.subscribe(canGuess => {
				if (!canGuess) {
					dispatch('gameOver', {});
				}
			});
		} else {
			notFound = true;
		}
	}
</script>


<div class="play-area">
	{#if !$canGuess}
		<div class="target-concelho">
			O concelho de hoje é: {$targetConcelho.concelho}, {$targetConcelho.distrito}
		</div>
	{/if}
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
	
</div>

<style>
	.guesses {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.not-found {
		color: red;
	}
	
	.target-concelho {
		margin-bottom: 0.5rem;
	}
</style>