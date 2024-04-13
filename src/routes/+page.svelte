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

<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>Document</title>
		<link rel="stylesheet" href="https://unpkg.com/@sakun/system.css" />
		<link rel="stylesheet" href="styles.css" />
	</head>

	<body>
		<div class="icon-grid">
			<Icon click={handleIconClick} />
		</div>

		{#each windows as window (window.title)}
			<Window
				title={window.title}
				close={() => closeWindow(window.title)}
				focus={() => focusWindow(window.title)}
			/>
		{/each}
	</body>
</html>

<style>
	/* html {
		margin: 0;
		padding: 0;
	} */
	body {
		margin: 0;
		padding: 0;
		width: 100vw;
		height: 100vh;
		overflow: hidden;
	}
	.icon-grid {
		display: grid;
		justify-content: end;
		/* grid-template-columns: auto 1fr 1fr; */
		padding: 3rem;
	}

	label {
		border: 1px black solid;
		background: white;
	}
</style>
