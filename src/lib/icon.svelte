<script>
	export let click;

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
	<img src="icons/File-Text.png" alt="" />
	<label>preface</label>
</div>

<style>
	.icon {
		width: 4rem;
		font-family: Chicago_12;
	}
	label {
		border: 1px black solid;
		background: white;
	}
</style>
