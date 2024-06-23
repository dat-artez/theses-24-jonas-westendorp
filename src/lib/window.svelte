<script lang="ts">
	import Markdown from '@magidoc/plugin-svelte-marked';
	import myMarkdown from '../lib/data/0-preface/index.md?raw';
	// props
	export let title: String;
	export let focus;
	export let close;
	export let isActive: boolean;

	// state
	let windowElement: HTMLDivElement;
	let isDragging: boolean = false;
	let window = { x: 5, y: 5, width: 780, height: 500 };
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
		<div class={isActive ? 'title-bar' : 'inactive-title-bar'} on:mousedown={moveWindow}>
			{#if isActive}
				<button aria-label="Close" class="close" on:click={close}></button>
			{/if}
			<h1 class="title">{title}</h1>
			<!-- <button aria-label="Resize" class="resize"></button> -->
		</div>
		<div class="separator"></div>

		<div class="window-pane">
			<Markdown source={myMarkdown} />
		</div>

		<div class="separator"></div>
		<div class="footer-bar">
			<button on:mousedown={resizeWindow} aria-label="Resize" class="resize"></button>
		</div>
	</div>
</div>

<style>
	.draggable-container {
		min-height: 12rem;
		position: absolute;
	}

	.footer-bar {
		display: flex;
		justify-content: end;
		position: relative;
		height: 22px;
	}
	::-webkit-scrollbar-track {
		border-left: 1px solid black !important;
	}
	.resize {
		cursor: se-resize;
		box-sizing: border-box;
		background:
			linear-gradient(#000, #000) left 58%,
			linear-gradient(180deg, #000 0, #000) 58% top;
		background-color: #fff;
		background-repeat: no-repeat;
		background-size:
			60% 2px,
			2px 60%;
		border: 1px solid #000;
		display: block;
		height: 22px;
		position: relative;
		width: 22px;
	}
	.window {
		width: 100%;
		height: 100%;
		margin: 0;
		display: grid;
		filter: drop-shadow(2px 2px 0px black);
		/* grid-template-rows: auto 1rem; */
	}
	.title-bar:hover {
		cursor: grab;
	}

	.window-pane {
		max-height: 80vh;
		max-width: 80vw;
		box-sizing: border-box;
		overflow-x: hidden;
	}

	:global(h1) {
		font-size: 2em;
	}
	:global(h2) {
		font-size: 1em;
	}
</style>
