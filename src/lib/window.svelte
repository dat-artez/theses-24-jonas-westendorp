<script lang="ts">
	// props
	export let title: String;
	export let focus;

	// state
	let windowElement: HTMLDivElement;
	let isDragging: boolean = false;
	let pos = { x: 5, y: 5 };
	let mouseOffsetBar: [number, number] = [0, 0];

	function handleClose() {
		title = title + 'a';
	}

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
	<div class="window">
		<div class="title-bar" on:mousedown={handleMouseDown}>
			<button aria-label="Close" class="close" on:click={handleClose}></button>
			<h1 class="title">{title}</h1>
			<button aria-label="Resize" class="resize"></button>
		</div>
		<div class="separator"></div>

		<div class="window-pane">Hello world!</div>
	</div>
</div>

<style>
	.draggable-container {
		position: absolute;
	}
	.title-bar:hover {
		cursor: grab;
	}
</style>
