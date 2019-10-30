## Overview

musicFlow is a web-app built with Svelte and designed to make it easier to make album recommendation flow-charts. In the past, I've encountered these charts being made manually with software like Paint or Photoshop so I decided to build an application to make it easier. This app also gave me an opportunity to learn Svelte so feedback is more than welcome on the code!

## Application Architecture

The app is pretty simple: the `Grid` component contains the HTML structure of the app (using CSS Grid) and also loads the `Main` component. This component is made up of two pieces: the `flowSidebar` div and the `flowMainArea` div.

### `flowSidebar`

`flowSidebar` contains a search input. Upon submitted input, it calls the [last.fm API](https://www.last.fm/api/) with the query to search for albums. It then displays all the JSON objects retrieved using Svelte's `each` loop. Because of the way Svelte works, whenever the array of results gets updated (`retrievedAlbums`), the `each` loop updates as well, making sure you see the latest results. However, every time you search, the array will get cleared since the whole idea of the flow chart (for now) is to focus on one band's discography. Images are displayed by filling in the retrieved object's image property into an `<img>` element.

### `flowMainArea`

When an image is clicked, its corresponding object receives the `selected: true` key/value pair. `flowMainArea` also has an `each` loop but this one only looks for objects with `selected` set to `true`. Again because of the way Svelete works, whenever an object gets updated, the `each` loop does as well, resulting in the image being moved to `flowMainArea`.

**Note**: right now, `flowMainArea` is tied to the same array as `flowSidebar`. Because of that, whenever you perform a new search (effectively clearing the array), whatever work you did in `flowMainArea` will get reset as well. Down the line, I plan to decouple these elements.

### Transitions

You'll note that when an image is moved from `flowSidebar` to `flowMainArea`, it does so smoothly. I've achieved this by using Svelte [deferred transitions](https://svelte.dev/tutorial/deferred-transitions), setting the elements which contain the images (`<li>`) to receive and send the elements. There's currently no transition on the images that get "left" in `flowSidebar` since the `animate` directive is doing weird stuff right now but I'll get that fixed as well down the line.

## Todo

Mostly writing this down for keeping myself honest but if someone out there (hello!) wants to take any of these tasks and contribute, please feel free to open a Pull Request!

* Finish the damn UI! It doesn't do anything right now except create a grid, add those fancy arrows!

* Decouple `flowSidebar` and `flowMainArea` to allow for more intricate flow charts (cross-band for example).

* Sort out `animate` on `flowSidebar`.

* Look into drag and drop for transition between the two elements (big task, probably falls under a v2 for the app).

* Do a better job of cleaning up the response from last.fm (remove duplicates, stop ignoring `null` and do something with, etc).

* Design a proper UI instead of the placeholder colors/title that's there right now.

* Get a proper domain.
