<script lang="ts">
	import Window from '$lib/window.svelte';
	import Icon from '$lib/icon.svelte';

	const windowData = [{ title: '0. Preface' }, { title: '1. Introduction' }];
	let windows = [...windowData];

	function focusWindow(title: string) {
		let focusWindow = null;
		//remove the focuswindow
		let newWindows = windows.filter((win) => {
			if (win.title === title) {
				focusWindow = win;
				return false;
			} else return true;
		});
		// add it back at the end
		windows = [...newWindows, focusWindow];
	}

	function closeWindow(title: string) {
		windows = windows.filter((win) => win.title !== title);
	}

	function addWindow(title: string) {
		let window = windowData.find((x) => x.title === title);

		windows = [...windows, window];
	}

	function handleIconClick() {
		addWindow('0. Preface');
	}
</script>

<div class="container">
	<nav>
		<h1>From Interaction to Transaction</h1>
	</nav>

	<div class="windowContent">
		<div class="icon-grid">
			<Icon title="0. Preface" icon="File-Text.png" click={handleIconClick} />
			<Icon title="1. Introduction" icon="Macintosh-Happy.png" click={handleIconClick} />
			<Icon title="2. Blue Screen of Death" icon="Macintosh-Unhappy.png" click={handleIconClick} />
		</div>

		<div class="window notes">Sticky notes</div>

		{#each windows as window, i (window.title)}
			<Window
				title={window.title}
				close={() => closeWindow(window.title)}
				focus={() => focusWindow(window.title)}
				isActive={i === windows.length - 1}
			/>
		{/each}
	</div>
</div>

<style>
	/* html {
		margin: 0;
		padding: 0;
	} */
	.windowContent {
		height: 100%;
		width: 100%;
		overflow: hidden;
		position: absolute;
	}

	.container {
		width: 100%;
		height: 100%;
		overflow: hidden;
		display: grid;
		grid-template-rows: 30px 1fr;
		background:
			linear-gradient(90deg, #fff 1px, transparent 1%) 50%,
			linear-gradient(#fff 1px, transparent 1%) 50%,
			#000;
		background-size: 2px 2px;
		/* position: absolute; */
		border: 0px solid black;
		border-radius: 10px;
		box-sizing: border-box;
	}
	nav {
		background: white;
		/* z-index: 999; */
		border-bottom: 1px solid black;
	}
	h1 {
		margin: 0;
		font-size: 1.1rem;
		padding: 0.1rem 1rem;
	}
	.icon-grid {
		display: grid;
		justify-content: end;
		/* grid-template-columns: auto 1fr 1fr; */
		padding: 3rem;
		gap: 3rem;
	}

	label {
		border: 1px black solid;
		background: white;
	}

	.notes {
		width: 400px;
		height: 400px;
	}
</style>
