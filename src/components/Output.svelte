<script>
	export let operations = '';
	export let result = 0;
</script>

<div class="output">
	<span class="operations">{operations}</span>
	<span class="result">{result}</span>
</div>

<style>
	.output {
		display: flex;
		flex-direction: column;
		width: 100%;
		background: #00000010;
		box-shadow: 0 0 6px 0 rgba(0, 0, 0, 0.35) inset;
		padding: 0.5em;
		height: 5em;
	}
	.operations {
		font-size: 0.85em;
	}
	.result {
		font-weight: bold;
		text-align: right;
	}
</style>
