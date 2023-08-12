<script>
	import Key from './Key.svelte';
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let physicalKeyboardLinked = false;

	function onKeydown(event) {
		dispatch('keydown', {
			value: event.detail.value
		});
	}
</script>

<div class="keyboard">
	<Key {physicalKeyboardLinked} name="9" x={3} y={2} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="8" x={2} y={2} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="7" x={1} y={2} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="6" x={3} y={3} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="5" x={2} y={3} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="4" x={1} y={3} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="3" x={3} y={4} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="2" x={2} y={4} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="1" x={1} y={4} on:keydown={onKeydown} />

	<Key {physicalKeyboardLinked} name="0" x={1} y={5} width={2} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="." x={3} y={5} on:keydown={onKeydown} />

	<Key {physicalKeyboardLinked} name="/" x={1} y={1} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="*" x={2} y={1} on:keydown={onKeydown} />
	<Key
		{physicalKeyboardLinked}
		key="Backspace"
		icon="backspace"
		x={3}
		y={1}
		width={2}
		on:keydown={onKeydown}
	/>
	<Key {physicalKeyboardLinked} name="-" x={4} y={2} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} name="+" x={4} y={3} height={2} on:keydown={onKeydown} />
	<Key {physicalKeyboardLinked} key="Enter" name="=" x={4} y={5} on:keydown={onKeydown} />
</div>

<style>
	.keyboard {
		width: fit-content;
		height: fit-content;
		display: grid;
		grid-gap: 0.5em;
		grid-template-columns: 3em 3em 3em 3em;
		grid-template-rows: 3em 3em 3em 3em 3em;
	}
</style>
