<script lang="ts">
	import Markdown from '@magidoc/plugin-svelte-marked';
	import myMarkdown from '../lib/data/0-preface/index.md?raw';
	// props
	export let title: String;
	export let focus;
	export let close;

	// state
	let windowElement: HTMLDivElement;
	let isDragging: boolean = false;
	let pos = { x: 5, y: 5 };
	let mouseOffsetBar: [number, number] = [0, 0];

	// handlers
	function handleMouseDown(e: MouseEvent) {
		e.stopPropagation();
		e.preventDefault();

		console.log('mouse on ' + title);

		isDragging = true;
		let rect = windowElement.getBoundingClientRect();
		let { x, y } = rect;
		let { clientX, clientY } = e;

		mouseOffsetBar = [clientX - x, clientY - y];

		focus();

		// listen for the other mouse events; and remove listeners once dragging has stopped
		document.body.addEventListener('mousemove', handleMouseMove);
		document.body.addEventListener(
			'mouseup',
			(e) => {
				isDragging = false;
				document.body.removeEventListener('mousemove', handleMouseMove);
			},
			{ once: true }
		);
	}

	function handleMouseMove(e: MouseEvent) {
		if (!isDragging) return;

		let { clientX, clientY } = e;
		let [xOffset, yOffset] = mouseOffsetBar;

		pos.x = clientX - xOffset;
		pos.y = clientY - yOffset;
	}
</script>

<div
	bind:this={windowElement}
	class="draggable-container"
	style={`top:${pos.y}px;left:${pos.x}px;`}
>
	<div class="window" on:click={focus}>
		<div class="title-bar" on:mousedown={handleMouseDown}>
			<button aria-label="Close" class="close" on:click={close}></button>
			<h1 class="title">{title}</h1>
			<button aria-label="Resize" class="resize"></button>
		</div>
		<div class="separator"></div>

		<div class="window-pane">
			<Markdown source={myMarkdown} />
		</div>
	</div>
</div>

<style>
	.draggable-container {
		position: absolute;
	}
	.title-bar:hover {
		cursor: grab;
	}

	.window-pane {
		max-height: 90vh;
	}

	:global(h1) {
		font-size: 2em;
	}
	:global(h2) {
		font-size: 1em;
	}
</style>
