<script>
import { fade } from 'svelte/transition';
import { quintOut } from 'svelte/easing';
import { crossfade } from 'svelte/transition';
import { flip } from 'svelte/animate';

const [send, receive] = crossfade({
  duration: d => Math.sqrt(d * 200),

  fallback(node, params) {
    const style = getComputedStyle(node);
    const transform = style.transform === 'none' ? '' : style.transform;

    return {
      duration: 400,
      easing: quintOut,
      css: t => `
        transform: ${transform} scale(${t});
        opacity: ${t}
      `
    };
  }
});
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

function remove(album) {
		retrievedAlbums = retrievedAlbums.filter(t => t !== album);
	}

function addAlbum(album, selected) {
  album.selected = selected
  remove(album);
  retrievedAlbums = retrievedAlbums.concat(album);
  console.log(album);
}

</script>

<div id="flowSidebar">
<input on:keydown={searchAlbum} bind:value={name} placeholder="Search for albums">
<ul id="albumList">
  {#if !name}
    <li></li>
  {:else}
    {#each retrievedAlbums.filter(t => !t.selected && t.name != '(null)') as album (album.name)}
        <li in:receive="{{key: album.name}}" out:send="{{key: album.name}}" on:click={() => addAlbum(album, true)}><img alt="{album.name}" src="{album.image[2]['#text']}" /><span>{album.name}</span></li>
    {/each}
  {/if}
</ul>
</div>
<div id="flowMainArea">
  <div id="selectedAlbums">
  <ul id="selectedList">
    {#each retrievedAlbums.filter(t => t.selected) as album, i (album.name)}
    <li class="item{i} selectedAlbum" in:receive="{{key: album.name}}" out:send="{{key: album.name}}" on:click={() => addAlbum(album, false)} ><span><img alt="{album.name}" src="{album.image[2]['#text']}" /><span>{album.name}</span></span></li>
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

.selectedAlbum {
  display: flex;
}

.selectedAlbum::before {
  content: url(images/arrowBase.svg);
  display: block;
  transform: rotate(180deg);
  margin-top: 20px;
  align-self: center;
  margin-left: -140px;
  margin-right: 10px;
}

.item0 {
  grid-row: 1;
  grid-column: 1;
}

.item0::before {
  display: none;
}

.item1 {
  grid-row: 1;
  grid-column: 2;
}

.item2 {
  flex-direction: column;
  grid-row: 2;
  grid-column: 1;
}

.item2::before {
  transform: rotate(270deg);
  margin-top: -110px;
  margin-bottom: 40px;
  margin-right: 0px;
  margin-left: 0px;
}

.item3 {
  grid-row: 1;
  grid-column: 3;
}

.item4 {
  flex-direction: column;
  grid-row: 3;
  grid-column: 1;
}

.item4::before {
  transform: rotate(270deg);
  margin-top: -110px;
  margin-bottom: 40px;
  margin-right: 0px;
  margin-left: 0px;
}

</style>
