<script lang="ts">
	export let source = `jon
sup
me`;

	let numberOfLines = source.split('\n').length;

	const KeyUp = (event: Event) => {
		const textarea = event.target as HTMLInputElement;
		numberOfLines = textarea.value.split('\n').length;
	};

	const KeyDown = (event: Event) => {
		const textarea = event.target as HTMLInputElement;

		if ((event as KeyboardEvent).key === 'Tab') {
			const start = textarea.selectionStart;
			const end = textarea.selectionEnd;

			if (start && end) {
				textarea.value = textarea.value.substring(0, start) + '\t' + textarea.value.substring(end);
				event.preventDefault();
			}
		}
	};
</script>

<center>
	<div class="editor">
		<div class="line-numbers">
			{#each Array(numberOfLines) as _}
				<span />
			{/each}
		</div>
		<textarea on:keyup={KeyUp} on:keydown={KeyDown} value={source} rows="15" />
	</div>
</center>

<style>
	.editor {
		display: inline-flex;
		gap: 10px;
		font-family: monospace;
		line-height: 21px;
		background: #282a3a;
		border-radius: 2px;
		padding: 20px 10px;
	}

	.line-numbers {
		width: 20px;
		text-align: right;
	}

	.line-numbers span {
		counter-increment: linenumber;
	}

	.line-numbers span::before {
		content: counter(linenumber);
		display: block;
		color: #506882 !important;
	}

	textarea {
		line-height: 21px;
		overflow-y: hidden;
		padding: 0;
		border: 0;
		background: #282a3a;
		color: #fff;
		min-width: 500px;
		outline: none;
		resize: none;
	}
</style>
