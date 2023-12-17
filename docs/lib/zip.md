# ZIP

To load a [ZIP archive](<https://en.wikipedia.org/wiki/ZIP_(file_format)>), use [`FileAttachment`](../javascript/files).

```js echo
const muybridge = FileAttachment("muybridge.zip").zip();
```

This returns a promise to a `ZipArchive`. This uses [JSZip](https://stuk.github.io/jszip/) under the hood.

```js echo
muybridge
```

The `filenames` property lists the paths of the files contained within the ZIP archive.

```js echo
muybridge.filenames
```

To pull out a single file from the archive, use the `archive.file` method. It returns a `FileAttachment` which you can use to load the file contents like any other file.

```js echo
muybridge.file("deer.jpeg").image({width: 320, alt: "A deer"})
```

That said, if you know the name of the file within the ZIP archive statically, you don’t need to load the ZIP archive; you can simply request the [file within the archive](../routing#archives) directly. The specified file is then extracted from the ZIP archive at build time.

```js echo
FileAttachment("muybridge/deer.jpeg").image({width: 320, alt: "A deer"})
```

For images and other media, you can simply use static HTML.

<img src="muybridge/deer.jpeg" width="320" alt="A deer">

```html
<img src="muybridge/deer.jpeg" width="320" alt="A deer">
```

One reason to load a ZIP archive is that you don’t know the files statically — maybe there are lots of files and you don’t want to enumerate them statically, or maybe you expect them to change over time and the ZIP archive is generated by a [data loader](../loaders). For example, maybe you want to display an arbitrary collection of images.

```js echo
Gallery(await Promise.all(muybridge.filenames.map((f) => muybridge.file(f).image())))
```

```js echo
function Gallery(images) {
  return html`<div style="
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 1rem;
">${images.map((i) => (i.style = 'width: 100%; height: 100%;', i))}</div>`;
}
```

To let the user download a ZIP archive, use the `download` attribute on a link.

<a href="muybridge.zip" download>
  <button>download zip</button>
</a>

```html
<a href="muybridge.zip" download>
  <button>download zip</button>
</a>
```