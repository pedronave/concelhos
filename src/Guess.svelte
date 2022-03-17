<script>
	export let guess;

	import {targetConcelho} from './stores';
    import GuessContainer from './GuessContainer.svelte';

	let distance = 0;
	let rad = 0;
	
	$: if (guess) {
		distance = calculateDistanceToGoal(guess); 
		rad = calculateRadToGoal(guess);
		
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

{#if guess}
	<GuessContainer guessText={guess.concelho} guessDistance={distance} guessAngle={rad} />
{:else}
	<GuessContainer />
{/if}