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
	let window = { x: 5, y: 5, width: 400, height: 500 };
	let mouseOffsetBar: [number, number] = [0, 0];

	// handlers
	function handleMouseDown(e: MouseEvent) {
		e.stopPropagation();
		e.preventDefault();

		isDragging = true;
		let rect = windowElement.getBoundingClientRect();
		let { x, y } = rect;
		let { clientX, clientY } = e;

		mouseOffsetBar = [clientX - x, clientY - y];

		focus();

		return { mouse: [clientX, clientY], offset: [clientX - x, clientY - y] };
	}

	function handleMouseMove(callback) {
		if (!isDragging) return;

		// listen for the other mouse events; and remove listeners once dragging has stopped
		document.body.addEventListener('mousemove', callback);
		document.body.addEventListener(
			'mouseup',
			(e) => {
				isDragging = false;
				document.body.removeEventListener('mousemove', callback);
			},
			{ once: true }
		);
	}

	function moveWindow(e: MouseEvent) {
		mouseOffsetBar = handleMouseDown(e).offset;

		let [xOffset, yOffset] = mouseOffsetBar;

		function move(e) {
			window.x = e.clientX - xOffset;
			window.y = e.clientY - yOffset;
		}

		handleMouseMove(move);
	}

	function resizeWindow(e: MouseEvent) {
		mouseOffsetBar = handleMouseDown(e).offset;
		let [xOffset, yOffset] = mouseOffsetBar;

		let rect = windowElement.getBoundingClientRect();

		function move(e) {
			window.width = e.clientX - rect.x;
			window.height = e.clientY - rect.y;
		}

		handleMouseMove(move);
	}
</script>

<div
	bind:this={windowElement}
	class="draggable-container"
	style={`top:${window.y}px;left:${window.x}px;width:${window.width}px;height:${window.height}px;`}
>
	<div class="window" on:click={focus}>
		<div class="title-bar" on:mousedown={moveWindow}>
			<button aria-label="Close" class="close" on:click={close}></button>
			<h1 class="title">{title}</h1>
			<button aria-label="Resize" class="resize"></button>
		</div>
		<div class="separator"></div>

		<div class="window-pane">
			<Markdown source={myMarkdown} />
		</div>
	</div>
	<div class="scale-corner" on:mousedown={resizeWindow}>x</div>
</div>

<style>
	.draggable-container {
		position: absolute;
	}
	.window {
		width: 100%;
		height: 100%;
		margin: 0;
		display: grid;
		grid-template-rows: auto 1rem;
	}
	.title-bar:hover {
		cursor: grab;
	}

	.scale-corner {
		width: 10px;
		height: 10px;
		background-color: red;
		position: absolute;
		right: 0;
		bottom: 0;
		cursor: se-resize;
	}

	.window-pane {
		max-height: 80vh;
		max-width: 80vw;
		box-sizing: border-box;
	}

	:global(h1) {
		font-size: 2em;
	}
	:global(h2) {
		font-size: 1em;
	}
</style>
