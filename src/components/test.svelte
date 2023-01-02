<script lang="ts">
	import { onMount } from 'svelte';

	export let source = `jon\nsup there is some other text here that I want to go across the lines and not word wrap another line etc boy!\nme\nfixin to do sumthin good`;

	let fontWidthElem: HTMLElement;
	let fontWidth: number;
	let charactersPerLine = 60;
	$: areaWidth = charactersPerLine * fontWidth;

	onMount(() => {
		fontWidth = fontWidthElem.getBoundingClientRect().width;
	});

	function wrapped(str: string, timesWrapped: number): number {
		if (str.length > charactersPerLine) {
			const firstLine = str.substring(0, charactersPerLine);
			const lastCharOfFirstLine = str[charactersPerLine - 1];
			const firstCharOfNextLine = str[charactersPerLine];
			if (lastCharOfFirstLine !== ' ' && firstCharOfNextLine !== ' ') {
				return wrapped(str.substring(firstLine.lastIndexOf(' ')).trim(), timesWrapped + 1);
			} else {
				return wrapped(str.substring(charactersPerLine).trim(), timesWrapped + 1);
			}
		} else {
			return timesWrapped;
		}
	}
</script>

<div class="editor">
	<div class="line-numbers">
		{#each source.split('\n') as subStr, index}
			<div class="line-nr" style:margin-bottom="calc(var(--line-height) * {wrapped(subStr, 0)})">
				{index + 1}
			</div>
		{/each}
	</div>

	<textarea bind:value={source} style:width="{areaWidth}px" rows="15" />

	<div id="font-width" bind:this={fontWidthElem}>a</div>
</div>

<style>
	:global(body) {
		padding: 0;
	}

	.editor {
		--line-height: 21px;
		position: relative;
		display: inline-flex;
		gap: 10px;
		padding: 20px 10px;
		font-family: monospace;
		line-height: var(--line-height);
		background: #282a3a;
		border-radius: 2px;
	}

	.line-nr {
		width: 20px;
		text-align: right;
		color: #506882 !important;
	}

	textarea {
		line-height: var(--line-height);
		overflow-y: hidden;
		margin: 0;
		padding: 0;
		border: 0;
		background: #282a3a;
		color: #fff;
		outline: none;
		resize: none;
	}

	#font-width {
		position: absolute;
		top: -1000px;
		left: -1000px;
		visibility: hidden;
	}
</style>
