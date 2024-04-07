<script lang="ts">
	import Window from '$lib/window.svelte';

	let windows = [{ title: '0. Preface' }, { title: '1. Introduction' }];

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
</style>
