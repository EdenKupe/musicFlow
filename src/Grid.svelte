<script>
	export let title;
	let name;
	
	function searchAlbum() {
		alert(`submitted ${pin}`);
	}
</script>

<div id="flowWrapper">
	<div id="flowHeader">
		<h1>Welcome to {title}!</h1>
	</div>
	<div id="flowSidebar">
		<input bind:value={name} placeholder="Search for albums">
		<p>You searched for {name || 'nothing, what are you waiting for?'}</p>
	</div>
	<div id="flowMainArea">
		
	</div>
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
	
	#flowSidebar {
		grid-column: 1 / span 1;
		grid-row: 2 / span 1;
		background-color: teal;
		display: flex;
		flex-direction: column;
		text-align:center;
		padding: 30px 20px 10px 20px;
	}
	
	#flowMainArea {
		grid-column: 2 / span 1;
		grid-row: 2 / span 1;
		background-color: blue;
	}
	
	#flowFooter {
		grid-column: 1 / span 2;
		grid-row: 3 / span 1;
		background-color: black;
	}
	
</style>