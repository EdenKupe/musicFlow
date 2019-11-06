<script>
export let className;
export let arrowClass;
export let effectKey;
export let albumName;
export let albumImage;
export let object;
export let toggle;
export let retrievedAlbums = [];
import { fade } from 'svelte/transition';
import { quintOut } from 'svelte/easing';
import { crossfade } from 'svelte/transition';
import { flip } from 'svelte/animate';

function remove(album) {
		retrievedAlbums = retrievedAlbums.filter(t => t !== album);
	}

function addAlbum(album, selected) {
	if (selected = 'true') {
  album.selected = true;
} else {
	album.selected = false;
}
  remove(album);
  retrievedAlbums = retrievedAlbums.concat(album);
  console.log(album);
}

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
</script>


<li class="{className} selectedAlbum" on:click={() => addAlbum({object}, {toggle})} ><span class="{arrowClass}"> <img alt="arrow" src="images/arrowBase.svg"/></span>
<span class="coverContainer"><span>{albumName}</span><img alt="{albumName}" src="{albumImage}" /></span></li>


<style>

.arrowNone {
  display: none;
}

.selectedAlbum {
  display: flex;
}

.coverContainer {
  display: flex;
  flex-direction: column;
}

.coverContainer span {
  position: absolute;
  color: #fff;
  width: 9%;
}

.arrowContainer {
  display: none;
  margin-left: -130px;
  margin-right: 20px;
}

.item0 {
  grid-row: 2;
  grid-column: 2;
}

.item0 .arrowContainer {
  display: none;
}

/* .item0::before {
  display: none;
} */

.item1 {
  grid-row: 2;
  grid-column: 3;
}

.item1 .arrowContainer {
  transform: rotate(180deg);
}

.item2 {
  grid-row: 2;
  grid-column: 1;
}

.item2 .arrowContainer {
  transform: rotate(180deg);
  margin-left: -100px;
}

.item3 {
  flex-direction: column;
  grid-row: 1;
  grid-column: 2;
}

.item3 .arrowContainer {
  transform: rotate(270deg);
  margin-top: -130px;
  margin-bottom: 60px;
  margin-right: 0px;
  margin-left: 0px;
}

.item4 {
  grid-row: 3;
  grid-column: 2;
}

.item4 .arrowContainer {
  transform: rotate(180deg);
}

.item5 .arrowContainer {
  transform: rotate(180deg);
  margin-left: -100px;
}

.item6 {
  flex-direction: column;
}

.item6 .arrowContainer {
  transform: rotate(270deg);
  margin-top: -130px;
  margin-bottom: 60px;
  margin-right: 0px;
  margin-left: 0px;
}

.item7 .arrowContainer {
  transform: rotate(180deg);
}

.item8 .arrowContainer {
  transform: rotate(180deg);
  margin-left: -100px;
}

</style>
