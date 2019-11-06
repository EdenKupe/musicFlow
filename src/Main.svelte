<script>
import Album from './Album.svelte'
let name;
export let retrievedAlbums = [];
function searchAlbum(e) {
    if (!e) e = window.event;
    var keyCode = e.keyCode || e.which;
    if (keyCode == '13') {
        // Enter pressed
        (async () => {
            const res = await fetch('https://ws.audioscrobbler.com/2.0/?method=album.search&album=' + name + '&api_key=ab0eaf7766018b2268b45604e0b2e15f&format=json&limit=20');
            retrievedAlbums = (await res.json()).results.albummatches.album;
            console.log(retrievedAlbums);
            this.value = '';
        })();
    }
}
</script>

<div id="flowSidebar">
<input on:keydown={searchAlbum} bind:value={name} placeholder="Search for albums">
<ul id="albumList">
    {#each retrievedAlbums.filter(t => !t.selected && t.name != '(null)') as album (album.name)}
      <Album object={album} toggle=true className='poolItem' arrowClass='arrowContainer' albumName={album.name} albumImage={album.image[2]["#text"]}/>
    {/each}
</ul>
</div>
<div id="flowMainArea">
  <div id="selectedAlbums">
  <ul id="selectedList">
    {#each retrievedAlbums.filter(t => t.selected) as album, i (album.name)}
    <Album object={album} toggle=false className="item + {i}" arrowClass='arrowContainer' albumName={album.name} albumImage={album.image[2]["#text"]}/>
    {/each}
  </ul>
  </div>
</div>

<style>

ul {
  list-style: none;
  margin-block-start: 0;
  margin-block-end: 0;
  padding-inline-start: 0;
}

#flowSidebar {
  grid-column: 1 / span 1;
  grid-row: 2 / span 1;
  background-color: #CFB3CD;
  display: flex;
  flex-direction: column;
  text-align:center;
  padding: 30px 20px 10px 20px;
}

#flowMainArea {
  grid-column: 2 / span 1;
  grid-row: 2 / span 1;
  background-color: #333333;
}

#albumList {
  display: flex;
  flex-wrap: wrap;
}

#selectedAlbums {
  height: 100%;
  display: flex;
  padding-left: 70px;
}

#selectedList {
  height: 100%;
  display: grid;
  grid-template-columns: 174px 174px 174px;
  grid-template-rows: 174px 174px 174px;
  grid-gap: 150px;
  list-style: none;
  flex-direction: column;
  margin-top: 120px;
}

#albumList li {
  text-align: left;
  flex: 1;
  font-size: 12px;
  color: #fff;
  margin-bottom: 10px;
  max-width: 200px;
}

.searchText {
  color: #666A86;
}

</style>
