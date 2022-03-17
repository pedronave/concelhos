<script>
	export let guessText;
	export let guessDistance;
	export let guessAngle;
</script>

<div class="guess">
	<span class="guess__text">{guessText ? guessText : ''}</span>
	<span class="guess__distance">{#if guessText} {guessDistance} km {/if}</span>
	<span class="guess__direction">
		{#if guessText}
			{#if guessDistance > 0}
				<svg style="transform: rotate({guessAngle}rad);" xmlns="http://www.w3.org/2000/svg" fill="none" height="24" width="24"  viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
					<path stroke-linecap="round" stroke-linejoin="round" d="M17 8l4 4m0 0l-4 4m4-4H3" />
				</svg>
			{:else}
				<svg xmlns="http://www.w3.org/2000/svg"  height="24" width="24" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
					<path stroke-linecap="round" stroke-linejoin="round" d="M9 12l2 2 4-4M7.835 4.697a3.42 3.42 0 001.946-.806 3.42 3.42 0 014.438 0 3.42 3.42 0 001.946.806 3.42 3.42 0 013.138 3.138 3.42 3.42 0 00.806 1.946 3.42 3.42 0 010 4.438 3.42 3.42 0 00-.806 1.946 3.42 3.42 0 01-3.138 3.138 3.42 3.42 0 00-1.946.806 3.42 3.42 0 01-4.438 0 3.42 3.42 0 00-1.946-.806 3.42 3.42 0 01-3.138-3.138 3.42 3.42 0 00-.806-1.946 3.42 3.42 0 010-4.438 3.42 3.42 0 00.806-1.946 3.42 3.42 0 013.138-3.138z" />
				</svg>
			{/if}
		{/if}
	</span>
</div>

<style>
.guess {
    display: grid;
    align-items: center;
	grid-template-columns: auto 65px 40px;
    min-height: 40px;
	border: 1px solid #ccc;
	border-radius: 6px;
	width: 100%;
	max-width: 350px;
}

.guess > * {
	height: 100%;
	display: inline-flex;
    align-items: center;
	border-right:  1px solid #ccc;
}

.guess > *:last-child {
	border-right: none;
}

.guess__text {
	padding: 0 0.5rem;
}

.guess__distance {
	justify-content: center;
}

.guess__direction {
	justify-content: center;
}
</style>