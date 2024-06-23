<script lang="ts">
	export let click;
	export let icon: 'File-Text.png' | 'Macintosh-Happy.png' | 'Macintosh-Unhappy.png';
	export let title: string;
	export let isActive: boolean;

	let selected = false;

	// function handleClick() {
	// 	click();
	// }
</script>

<div
	class="icon"
	on:dblclick={click}
	on:click={() => (selected = true)}
	style={selected && 'filter: invert(1);'}
>
	<img src={`icons/${icon}`} alt="" />
	<div class="label-container">
		<span class="label">{title}</span>
	</div>
</div>

<style>
	.icon {
		cursor: pointer;
		width: 70px;
		font-family: Chicago_12;
		position: relative;
		display: grid;
		place-items: center;
		/* background: white; */
	}
	.label-container {
		position: absolute;
		top: 3.5rem;
		font-size: 1.2rem;
		background: white;
		font-family: Monaco;
		padding: 0 0.5rem;
		text-align: center;
		margin: 0 -1rem;
	}

	.label {
		/* position: absolute; */
	}
</style>
