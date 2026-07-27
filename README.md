# sagsan.com

Personal technical blog and public lab notebook for Sagar Sanghani, published at [sagsan.com](https://sagsan.com).

The site documents hands-on experiments in applied AI, machine learning, and data systems: what was built, how it was evaluated, which tradeoffs mattered, what failed, and what was learned. It is intentionally not a conventional résumé or corporate portfolio.

## Mission

The professional objective is to make Sagar an exceptional candidate for AI Technical Program Manager roles. The site complements the résumé and LinkedIn rather than duplicating them:

- The résumé establishes formal TPM experience.
- The site demonstrates technical fluency, product judgment, program thinking, execution, and clear communication.
- Personal projects provide public evidence without exposing confidential employer work.

The intended reader should come away thinking that Sagar can lead complex AI programs because he understands the technology, makes sound product and engineering tradeoffs, communicates at multiple levels, and builds systems himself.

## Editorial identity

The site is an edited lab notebook for applied AI—not generic thought leadership and not a collection of isolated tutorials. Content falls loosely into three groups:

- **Experiments:** Bounded technical questions with code and measurable results.
- **Systems:** Larger, multi-part builds covering architecture, evaluation, operations, and iteration.
- **Notes:** Shorter technical observations, lessons, and occasional musings.

Substantial articles should work at two levels. A concise opening should explain the problem, result, and important decision to a broad technical or executive reader. The rest should give engineers enough implementation detail to inspect the reasoning.

## Content

The initial set of migrated personal-project articles includes:

- Forecasting in SQL: Building an Air Quality Model in BigQuery
- Mapping r/conspiracy via Vertex AI & BigQuery ML
- Iterating ML Models Using Weights & Biases
- Beyond Keywords: Unlock Smarter AI with Semantic Search
- AI Powered Small-Business ChatBot
- Building Snoop-Speare
- High-Performance Image Classification with PyTorch and Hugging Face

Future flagship series are expected to cover:

- PMLE
- An algorithmic stock-trading system
- Computer-vision golf analysis

These projects should demonstrate both technical implementation and AI TPM judgment through success criteria, sequencing, dependencies, evaluation, risk controls, cost/latency/quality tradeoffs, limitations, and retrospectives.

## Planned site structure

- A short About introduction with social and contact links
- PaperMod's standard chronological article list
- A Posts page
- A minimal custom footer without theme attribution

Primary navigation should remain small: the site title links Home, with Posts as the only main navigation item. Tags and series can organize the growing archive without adding permanent navigation clutter.

## Publishing approach

The seven original Medium articles have been recreated as Markdown on `sagsan.com`, with their original publication dates and relevant notebooks, code, datasets, and demos preserved. Updating their Medium canonical URLs to point to the corresponding `sagsan.com` posts remains a post-launch task.

New articles should originate on `sagsan.com` and may then be syndicated to Medium with `sagsan.com` retained as the canonical source.

Pushes to `main` are built and deployed automatically through GitHub Pages. The custom domain is configured in the repository's Pages settings, while DNS remains managed in Porkbun.

## Stack

- [Hugo](https://gohugo.io/) static site generator
- [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme with restrained, repository-local overrides
- GitHub Pages hosting and HTTPS
- Porkbun DNS for `sagsan.com`
- `sagar@sagsan.com`, forwarded to the owner's Gmail account

PaperMod provides the homepage, article presentation, responsive design, typography, colors, and dark mode. The site intentionally stays close to PaperMod's plain defaults, with content configuration and a minimal custom footer instead of a custom visual system.

## Local development

Start the local site with:

```sh
hugo server -D
```

Then open the local URL printed by Hugo, normally `http://localhost:1313/`.

Create a production build with:

```sh
hugo --minify
```

## Project status

The Hugo and PaperMod foundation, homepage, Posts page, custom footer, and all seven original Medium articles are complete. Visual QA, final editorial review, and deployment remain. See the [project roadmap](docs/roadmap.md) for the full checklist.

## Cost target

Hosting, SSL, and site tooling should remain free. The only expected recurring expense is the domain registration, approximately $11 per year.
