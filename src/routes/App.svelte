<script>
	import Calculator from '../components/Calculator.svelte';

	let focusedCalculator = 0;

	function focus(event) {
		focusedCalculator = event.detail.index;
	}
</script>

<div class="app">
	{#each Array(4) as _, i}
		<Calculator index={i} focused={focusedCalculator == i} on:focus={focus} />
	{/each}
</div>

<style>
	.app {
		width: 100vw;
		height: 100vh;
		max-width: 100vw;
		max-height: 100vh;
		overflow: hidden;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		flex-wrap: wrap;
		gap: 4em;
		background-image: linear-gradient(-225deg, #defdf3 0%, #f7dbf1 100%);
		font-family: Arial, Helvetica, sans-serif;
	}
</style>
