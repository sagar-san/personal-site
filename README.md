# sagsan.com

Golf swing analysis Parts 1 and 2 include local MP4 demos using the `video` shortcode, with playback controls, poster images, and no automatic video preload. Part 3 remains a draft.

The autonomous stock trader retrospective includes a recorded demo, historical trade screenshot, and links to the public dashboard and source code. Live orders stopped in mid-August 2026.

## Stack

- [Hugo](https://gohugo.io/) static site generator
- [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme with repository-local overrides
- GitHub Pages hosting and HTTPS
- Porkbun DNS for `sagsan.com`

## Local development

Start the local site:

```sh
hugo server -D
```

Then open the URL printed by Hugo, normally `http://localhost:1313/`.

Create a production build:

```sh
hugo --minify
```
