# sagsan.com

Golf swing analysis Parts 1 and 2 embed YouTube demos using Hugo’s `youtube` shortcode. Part 3 remains a draft.

The autonomous stock trader retrospective includes a recorded demo, historical trade screenshot, and links to the public dashboard and source code. Live orders stopped in mid-August 2026.

## Stack

- [Hugo](https://gohugo.io/) static site generator
- [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme with repository-local overrides
- GitHub Pages hosting and HTTPS
- Porkbun DNS for `sagsan.com`

The homepage social links are ordered email, LinkedIn, Kaggle, GitHub, Medium, then RSS. A scoped override in `assets/css/extended/home.css` removes the introduction’s minimum height and reduces the gap before the posts.

Article lists use compact, uncropped previews from page-bundle cover images, styled in `assets/css/extended/list-covers.css`. Golf Parts 1 and 2 use their demo stills; the stock trader uses its trade log; image classification uses its existing Fashion-MNIST cover. New covers are hidden on individual articles to avoid duplicating their media.

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

The two-part Transfer Learning series is enabled for publication, with its original URLs preserved and publication dates set to July 1, 2026 (Part I) and July 28, 2026 (Part II). Part I reports the saved 10,000-image confusion matrix; Part II compares models on the author-confirmed 600-image evaluation set.
