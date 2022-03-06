<script>
	export let guess;
	import {targetConcelho} from './stores';

	const distance = calculateDistanceToGoal(guess); 
	const rad = calculateRadToGoal(guess);
	
	function calculateDistanceToGoal(guess) {
		var R = 6371; // Radius of the earth in km
		var dLat = deg2rad(guess.y-$targetConcelho.y);  // deg2rad below
		var dLon = deg2rad(guess.x-$targetConcelho.x); 
		var a = 
			Math.sin(dLat/2) * Math.sin(dLat/2) +
			Math.cos(deg2rad($targetConcelho.y)) * Math.cos(deg2rad(guess.y)) * 
			Math.sin(dLon/2) * Math.sin(dLon/2)
			; 
		var c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a)); 
		var d = R * c; // Distance in km
		return Math.round(d);
	}

	function calculateRadToGoal(guess) {
		const deltax = $targetConcelho.x - guess.x;
		const deltay = guess.y - $targetConcelho.y;
		const angle = Math.atan2(deltay, deltax);

		return angle;
	}

	function deg2rad(deg) {
		return deg * (Math.PI/180)
	}
	
</script>

<div class="guess">
	<p>
        {guess.concelho} - {distance} km
    </p>
    {#if distance > 0}
        <svg style="transform: rotate({rad}rad);" xmlns="http://www.w3.org/2000/svg" fill="none" height="24" width="24"  viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M17 8l4 4m0 0l-4 4m4-4H3" />
        </svg>
    {:else}
        <svg xmlns="http://www.w3.org/2000/svg"  height="24" width="24" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9 12l2 2 4-4M7.835 4.697a3.42 3.42 0 001.946-.806 3.42 3.42 0 014.438 0 3.42 3.42 0 001.946.806 3.42 3.42 0 013.138 3.138 3.42 3.42 0 00.806 1.946 3.42 3.42 0 010 4.438 3.42 3.42 0 00-.806 1.946 3.42 3.42 0 01-3.138 3.138 3.42 3.42 0 00-1.946.806 3.42 3.42 0 01-4.438 0 3.42 3.42 0 00-1.946-.806 3.42 3.42 0 01-3.138-3.138 3.42 3.42 0 00-.806-1.946 3.42 3.42 0 010-4.438 3.42 3.42 0 00.806-1.946 3.42 3.42 0 013.138-3.138z" />
        </svg>
    {/if}
</div>

<style>
.guess {
    display: inline-flex;
    align-items: center;
    margin-bottom: 1rem;
    flex-grow: 0;
}

.guess svg {
    margin-left: 0.75rem;
}
</style>