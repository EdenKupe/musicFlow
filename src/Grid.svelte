<script>
	export let title;
	let name;
	let URL;
	let retrievedAlbums = [];

	function searchAlbum(e) {
	if (!e) e = window.event;
  var keyCode = e.keyCode || e.which;
	if (keyCode == '13'){
      // Enter pressed
				(async () => {
			    const res = await fetch('http://ws.audioscrobbler.com/2.0/?method=album.search&album=' + name + '&api_key=ab0eaf7766018b2268b45604e0b2e15f&format=json&limit=20');
			    retrievedAlbums = (await res.json()).results.albummatches.album;
			    console.log(retrievedAlbums)
			  })();
		}
    }
</script>

<div id="flowWrapper">
	<div id="flowHeader">
		<h1>Welcome to {title}!</h1>
	</div>
	<div id="flowSidebar">
		<input on:keydown={searchAlbum} bind:value={name} placeholder="Search for albums">
		<p>Press enter to search for {name || 'nothing, what are you waiting for?'}</p>
		<ul id="albumList">
			{#if !name}
			<li></li>
			{:else}
			{#each retrievedAlbums as album, i}
			{#if album.name != '(null)'}
			<li><img alt="{album.name}" src="{album.image[2]['#text']}" /> {album.name} </li>
			{/if}
			{/each}
			{/if}
		</ul>
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
		background-color: #344055;
		display: flex;
		flex-direction: column;
		text-align:center;
		padding: 30px 20px 10px 20px;
	}

	#flowMainArea {
		grid-column: 2 / span 1;
		grid-row: 2 / span 1;
		background-color: #CFB3CD;
	}

	#flowFooter {
		grid-column: 1 / span 2;
		grid-row: 3 / span 1;
		background-color: #266DD3;
	}

	ul {
		display: flex;
		flex-wrap: wrap;
		list-style: none;
		margin-block-start: 0;
		margin-block-end: 0;
		padding-inline-start: 0;
	}

	ul li {
		text-align: left;
		flex: 1;
		font-size: 12px;
		color: #fff;
		margin-bottom: 10px;
	}

	#flowSidebar p {
		color: #fff;
	}

</style>
