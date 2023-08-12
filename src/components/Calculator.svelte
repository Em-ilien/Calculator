<script>
	import Keyboard from './Keyboard.svelte';
	import Output from './Output.svelte';
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let focused = true;

	let operations = [];
	let result = 0;

	let mousedown = false;
	let pos = {
		x: 'unset',
		y: 'unset'
	};
	let position = 'initial';

	function onKeydown(event) {
		if (!focused) return;

		if (event.detail.value === 'Backspace') {
			if (operations[operations.length - 1].length > 1) {
				operations[operations.length - 1] = operations[operations.length - 1].slice(0, -1);
			} else {
				operations = operations.slice(0, -1);
			}
			console.log(operations);
			computeResult();
			return;
		}

		operations = [...operations, event.detail.value];

		//regrouper les elements chiffres adjacents
		for (let i = 0; i < operations.length - 1; i++) {
			if (isNumeric(operations[i]) && isNumeric(operations[i + 1])) {
				operations[i] = operations[i] + operations[i + 1];
				operations.splice(i + 1, 1);
			}
		}

		//si deux opérateurs adjacents, on garde le dernier
		for (let i = 0; i < operations.length - 1; i++) {
			if (!isNumeric(operations[i]) && !isNumeric(operations[i + 1])) {
				operations.splice(i, 1);
			}
		}

		console.log(operations);
		computeResult();
	}

	function computeResult() {
		function calculateExpression(expression) {
			let numbers = [];
			let operators = [];
			let currentNumber = '';

			for (const item of expression) {
				if (item === '+' || item === '-' || item === '*' || item === '/') {
					numbers.push(parseFloat(currentNumber));
					operators.push(item);
					currentNumber = '';
				} else {
					currentNumber += item;
				}
			}

			numbers.push(parseFloat(currentNumber));

			// Effectuer les calculs des multiplications et divisions en premier
			for (let i = 0; i < operators.length; i++) {
				if (operators[i] === '*' || operators[i] === '/') {
					const operator = operators[i];
					const num1 = numbers[i];
					const num2 = numbers[i + 1];

					if (operator === '*') {
						numbers.splice(i, 2, num1 * num2);
						operators.splice(i, 1);
						i--;
					} else if (operator === '/') {
						numbers.splice(i, 2, num1 / num2);
						operators.splice(i, 1);
						i--;
					}
				}
			}

			// Effectuer les calculs des additions et soustractions
			let result = numbers[0];
			for (let i = 0; i < operators.length; i++) {
				if (operators[i] === '+') {
					result += numbers[i + 1];
				} else if (operators[i] === '-') {
					result -= numbers[i + 1];
				}
			}

			return result;
		}

		let operationsCopy = [...operations];

		//si opationsCopy débute par un opérateur, on ajoute 0 au début
		if (!isNumeric(operationsCopy[0])) {
			operationsCopy.unshift('0');
		}

		//si opationsCopy finit par un opérateur, on ajoute 0 à la fin
		if (!isNumeric(operationsCopy[operationsCopy.length - 1])) {
			operationsCopy.push('0');
		}

		result = Number(calculateExpression(operationsCopy.join('')));
	}

	/**
	 * Vérifie si la valeur passée en paramètre est un nombre nombre, un point seul, ou un nombre avec un point
	 * @param value
	 */
	function isNumeric(value) {
		return /^-?\d+\.?\d*$/.test(value);
	}

	function handleMouseMove(event) {
		if (!mousedown) return;

		if (pos.x === 'unset' && pos.y === 'unset') {
			let el = event.target;
			while (el.classList.contains('calculator') === false) {
				el = el.parentElement;
			}
			pos = {
				x: el.offsetLeft,
				y: el.offsetTop
			};
		}

		pos = {
			x: pos.x + event.movementX,
			y: pos.y + event.movementY
		};
		position = 'absolute';
	}

	let zIndex = 1;

	function computeZIndex() {
		document.querySelectorAll("*[style*='z-index:']").forEach((el) => {
			if (el.style.zIndex >= zIndex) {
				zIndex = parseInt(el.style.zIndex) + 1;
			}
		});
	}

	export let index = null;

	function onMousedown() {
		mousedown = true;
		computeZIndex();
		dispatch('focus', {
			index: index,
			zIndex: zIndex
		});
	}
</script>

<div
	class="calculator"
	on:mousedown={onMousedown}
	on:mouseup={() => (mousedown = false)}
	style="position: {position}; left: {pos.x}px; top: {pos.y}px; z-index: {zIndex};"
>
	<Output operations={operations.join('') + ''} {result} />
	<Keyboard on:keydown={onKeydown} physicalKeyboardLinked={focused} />
</div>

<svelte:window on:mousemove={handleMouseMove} />

<style>
	.calculator {
		display: flex;
		flex-direction: column;
		width: fit-content;
		height: fit-content;
		gap: 1.5em;
		align-items: center;
		box-shadow: 0.25em 0.25em 0.5em 0.125em rgba(0, 0, 0, 0.35);
		border: 2px solid #00000025;
		border-bottom: none;
		border-right: none;
		border-radius: 0.5em;
		padding: 1.25em;
		background: #ffffffaa;
		position: absolute;
		user-select: none;
	}
</style>
