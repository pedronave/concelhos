<script>
	import { guesses, concelhos, targetConcelho, canGuess } from "./stores.js";
	import { createEventDispatcher } from 'svelte';
	import Guess from './Guess.svelte';
	
	let notFound = false;
	let currentGuess = '';
	let options = [];

	const dispatch = createEventDispatcher();

	const updateOptions = (event) => {
		const cleanedUpValue = event.target.value.toLowerCase().replace(/\s+/g, ' ');
		if (cleanedUpValue.trim().length === 0) {
			options = [];
		} else {
			options = concelhos.filter(x => {
				const cleanedUpName = x.concelho.toLowerCase();
				return cleanedUpName.includes(cleanedUpValue);
			}).sort((a, b) => a.concelho.length - b.concelho.length).slice(0, 5);
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
    <img src={'svg/' + $targetConcelho.id + '.svg'} alt="Imagem do município" />
	{#if !$canGuess}
		<div class="target-concelho">
			O concelho de hoje é: {$targetConcelho.concelho}, {$targetConcelho.distrito}
		</div>
	{/if}
	<div class="guesses">
		<Guess guess={$guesses.length > 0 ? $guesses[0] : null}></Guess>
		<Guess guess={$guesses.length > 1 ? $guesses[1] : null}></Guess>
		<Guess guess={$guesses.length > 2 ? $guesses[2] : null}></Guess>
		<Guess guess={$guesses.length > 3 ? $guesses[3] : null}></Guess>
		<Guess guess={$guesses.length > 4 ? $guesses[4] : null}></Guess>
		<Guess guess={$guesses.length > 5 ? $guesses[5] : null}></Guess>
	</div>
	<form class="guess-form" id="guess-form" on:submit|preventDefault|stopPropagation>
		<input autocomplete="off" type="text" id="guess-input" placeholder="Concelho" on:input={(e) => {notFound = false; updateOptions(e)}} bind:value={currentGuess} disabled={!$canGuess}>
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
		margin-bottom: 0.75rem;
	}

	.not-found {
		color: red;
	}

	.target-concelho {
		margin-bottom: 0.5rem;
	}

	.guess-form {
		display: inline-grid;
		width: 300px;
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
		border-bottom: 1px rgb(200, 200, 200) solid;
		min-height: 40px;
	}

	.guess-options button:active, button:hover, button:focus {
		background-color: rgb(223, 223, 223);
	}
</style>