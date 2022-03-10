<script>
	import { guesses, concelhos, targetConcelho, canGuess } from "./stores.js";
	import { createEventDispatcher } from 'svelte';
	import Guess from './Guess.svelte';
	
	let notFound = false;
	let currentGuess = '';
	
	let options = [];

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

	const updateOptions = (event) => {
		const cleanedUpValue = event.target.value.toLowerCase().replace(/\s+/g, ' ');
		if (cleanedUpValue.trim().length === 0) {
			options = [];
		} else {
			options = concelhos.filter(x => {
			const cleanedUpName = x.concelho.toLowerCase();
			return cleanedUpName.includes(cleanedUpValue);
		});
		}
		
	};

	const chooseGuess = (evt, concelho) => {
		evt.preventDefault();
		guesses.addGuess(concelho);
			currentGuess = '';
			options = [];
			canGuess.subscribe(canGuess => {
				if (!canGuess) {
					dispatch('gameOver', {});
				}
			});
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
	<form class="guess-form" id="guess-form">
		<input autocomplete="off" type="text" id="guess-input" on:input={(e) => {notFound = false; updateOptions(e)}} bind:value={currentGuess} disabled={!$canGuess}>
		{#if $canGuess && options.length > 0}
			<div class="guess-options">
				{#each options as option}
					<button on:click={(e) => chooseGuess(e, option)}>{option.concelho}{#if concelhos.filter(c => c.concelho === option.concelho).length > 1}, {option.distrito}{/if} </button>
				{/each}
			</div>
		{/if}
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

	.guess-form {
		display: inline-grid;

	}
	.guess-options {
		display: inline-flex;
		flex-direction: column;
		border: 1px black solid;
	}

	.guess-options button {
		background-color: white;
		border: none;
		padding: 0;
		margin: 0;
		cursor: pointer;
		padding: 0.5rem;
	}

	.guess-options button:active, button:hover, button:focus {
		background-color: rgb(197, 197, 197);
	}
</style>