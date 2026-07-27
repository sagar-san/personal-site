# sagsan.com

Personal website and blog for Sagar, published at [sagsan.com](https://sagsan.com).

The site is intended to be a fast, distraction-free corner of the web for a short bio, selected projects, and occasional writing. Its visual direction is deliberately simple: a raw, text-first indie-web aesthetic without unnecessary tracking or interface clutter.

## Planned stack

- [Hugo](https://gohugo.io/) static site generator
- [Hugo Bear Blog](https://github.com/janraasch/hugo-bearblog) theme by Jan Raasch
- GitHub Pages hosting and HTTPS
- Porkbun DNS for `sagsan.com`
- `sagar@sagsan.com`, forwarded to the owner's Gmail account

## Local development

The Hugo project has not been scaffolded yet. Once it is in place, the expected development loop will be:

```sh
hugo server -D
```

Then open the local URL printed by Hugo, normally `http://localhost:1313/`.

## Project status

The domain and email-forwarding phase is complete. Site implementation and deployment remain. See the [project roadmap](docs/roadmap.md) for the full checklist.

## Repository layout

```text
.
├── .gitignore      # Local and generated files excluded from Git
├── AGENTS.md       # Guidance for coding agents working in this repository
├── README.md       # Project overview
└── docs/
    └── roadmap.md  # Build and launch plan
```

This layout will expand when the Hugo site is created.

## Cost target

Hosting, SSL, and site tooling should remain free. The only expected recurring expense is the domain registration, approximately $11 per year.
