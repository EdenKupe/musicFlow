<script>
import Main from './Main.svelte';
export let title;
</script>

<div id="flowWrapper">
	<div id="flowHeader">
		<h1>Welcome to {title}!</h1>
	</div>
<Main />
	<div id="flowFooter">

	</div>
</div>

<style>
	#flowWrapper {
		display: grid;
		grid-template-columns: 2fr 4fr;
		grid-template-rows: 1fr 8fr 1fr;
		height: 100vh;
	}

	#flowHeader {
		grid-column: 1 / span 2;
		grid-row: 1 / span 1;
	}

	#flowFooter {
		grid-column: 1 / span 2;
		grid-row: 3 / span 1;
		background-color: #266DD3;
	}

</style>
