<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let name = '';
	export let key = name;
	export let icon = null;
	export let physicalKeyboardLinked = false;

	export let width = 1;
	export let height = 1;
	export let x = 1;
	export let y = 1;

	let active = false;

	function onKeydown(event) {
		if (!physicalKeyboardLinked) return;
		if (event.key != key) return;

		active = true;
		setTimeout(() => {
			active = false;
		}, 100);

		dispatch('keydown', {
			value: event.key
		});
	}

	function onClick() {
		dispatch('keydown', {
			value: key
		});
	}
</script>

<div
	class="key{active ? ' active' : ''}"
	style="grid-row-start: {y}; grid-column-start: {x}; grid-row-end: {y +
		height}; grid-column-end: {x + width};"
	on:click={onClick}
>
	{#if icon}
		<img src="icons/{icon}.png" alt="Icône" style="max-width: 75%; max-height: 75%;" />
	{:else}
		<span>{name}</span>
	{/if}
</div>

<svelte:window on:keydown|preventDefault={onKeydown} />

<style>
	.key {
		cursor: pointer;
		box-shadow: 0 0 0.125em 0.025em #00000080;
		font-size: 1.5em;
		border-radius: 0.125em;
		display: flex;
		justify-content: center;
		align-items: center;
		user-select: none;
	}
	.key:hover {
		box-shadow: 0 0 0.25em 0.05em #000000b0;
	}
	.key:active,
	.key.active {
		box-shadow: 0 0 0.125em 0.025em #00000080 inset;
	}
	.key:active span,
	.key.active span {
		transform: scale(0.95);
	}
</style>
