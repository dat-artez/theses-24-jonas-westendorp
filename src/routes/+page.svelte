<script lang="ts">
	export const prerender = true;
	import Window from '$lib/window.svelte';
	import Icon from '$lib/icon.svelte';
	import preface from '../lib/data/0-preface/index.md?raw';
	import intro from '../lib/data/1-introduction/index.md?raw';
	import transaction from '../lib/data/2-transaction/index.md?raw';
	import ifvalue from '../lib/data/3-interface-value/index.md?raw';
	import smalltalk from '../lib/data/4-smalltalk/index.md?raw';
	import innerface from '../lib/data/5-innerface/index.md?raw';
	import faceless from '../lib/data/6-faceless/index.md?raw';
	import interspace from '../lib/data/7-interspace/index.md?raw';
	import bsod from '../lib/data/8-bsod/index.md?raw';
	import bibliography from '../lib/data/bibliography/index.md?raw';

	const windowData = [
		{ title: '0. Preface', markdown: preface, icon: 'File-Text.png' },
		{ title: '1. Introduction', markdown: intro, icon: 'Macintosh-Happy.png' },
		{ title: '2. From Interaction to Transaction', markdown: transaction, icon: 'File-Text.png' },
		{ title: '3. Interface Value', markdown: ifvalue, icon: 'File-Text.png' },
		{ title: '4. No More SmallTalk', markdown: smalltalk, icon: 'File-Text.png' },
		{ title: '5. The Inner-Face', markdown: innerface, icon: 'File-Text.png' },
		{ title: '6. The Faceless', markdown: faceless, icon: 'File-Text.png' },
		{ title: '7. Interspace', markdown: interspace, icon: 'File-Text.png' },
		{ title: '8. The Blue Screen of Death', markdown: bsod, icon: 'File-Text.png' },
		{ title: 'Bibliography', markdown: bibliography, icon: 'Macintosh-Unhappy.png' }
	];
	let windows = [...windowData.reverse()];

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

	function handleIconClick(id) {
		addWindow(id);
	}
</script>

<div class="container">
	<nav>
		<h1>From Interaction to Transaction</h1>
	</nav>

	<div class="windowContent">
		<div class="icon-grid">
			{#each windowData as window, i (window.title)}
				<Icon
					title={window.title}
					icon={window.icon}
					click={() => handleIconClick(window.title)}
					isActive={i === windows.length - 1}
				/>
			{/each}
		</div>

		<div class="window notes">
			<h1>From Interaction to Transaction</h1>
			<h2>Examining the Interface and its Side Effects in Today’s Interface Economy</h2>

			Written by Jonas Westendorp, 2024
		</div>

		{#each windows as window, i (window.title)}
			<Window
				title={window.title}
				close={() => closeWindow(window.title)}
				focus={() => focusWindow(window.title)}
				isActive={i === windows.length - 1}
				markdown={window.markdown}
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
		display: flex;
		flex-direction: column;
		flex-wrap: wrap;
		align-content: end;
		justify-content: end;
		/* grid-template-columns: auto 1fr 1fr; */
		padding: 3rem;
		gap: 3rem;
		height: 100%;
		box-sizing: border-box;
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
