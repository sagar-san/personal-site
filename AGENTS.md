# AGENTS.md

## Purpose

This repository contains Sagar Sanghani's personal technical blog at `sagsan.com`. The intended stack is Hugo with PaperMod, deployed through GitHub Pages.

The site is a writing-first public lab notebook for applied AI, machine learning, and data systems. It is not primarily a résumé, corporate portfolio, or generic tutorial blog. Its professional objective is to make Sagar an exceptional AI Technical Program Manager candidate by showing technical fluency, product and program judgment, hands-on execution, intellectual honesty, and clear communication.

## Working agreement

Agents are authorized to inspect, create, edit, move, and delete files within this repository when doing so is necessary to complete the user's request. They may also run local development tools, formatters, tests, builds, and Git commands whose effects remain within this repository.

Ask for the user's permission before any action that affects resources outside this repository, including:

- creating, changing, or deleting remote services or cloud resources;
- purchasing or configuring domains, DNS, email forwarding, or paid services;
- installing system-wide software or changing machine-level configuration;
- reading from or writing to files outside the repository;
- publishing, deploying, opening pull requests, or otherwise changing remote repositories, except for pushes to `git@github.com:sagar-san/personal-site.git`;
- sending messages or acting as the user with an external account.

An explicit user request for one of those actions counts as permission only for the stated action and scope. Follow platform approval requirements even when this file grants repository-local authority.

Agents may push repository changes to `git@github.com:sagar-san/personal-site.git` without asking for permission. This standing authorization applies only to pushes to that repository; it does not authorize force-pushing, rewriting remote history, changing repository settings, opening pull requests, publishing releases, or deploying the site.

## Positioning

The intended reader should conclude that Sagar can lead complex AI programs because he:

- understands the underlying technology;
- turns ambiguous ideas into scoped, measurable work;
- makes sound product and engineering tradeoffs;
- considers data, evaluation, safety, cost, latency, operations, and failure modes;
- communicates outcomes concisely while providing technical depth;
- builds and evaluates systems himself.

The résumé and LinkedIn establish formal TPM experience. Do not force confidential workplace case studies into the site. Use personal projects as public evidence of both technical depth and program thinking.

## Content strategy

Use three loose content types:

- **Experiments:** Bounded questions with implementation details and measurable results.
- **Systems:** Larger builds that may span architecture, evaluation, operations, and retrospectives.
- **Notes:** Short technical observations, lessons, or musings.

Treat flagship work such as PMLE, algorithmic stock trading, and computer-vision golf analysis as evidence of technical implementation and AI TPM judgment, not as product marketing.

## Article standards

Before drafting, editing, or reviewing any article, read and follow the repository's [article review skill](docs/article-review/SKILL.md). This includes structural reviews, technical reviews, copyedits, rewrites, migrations, and pre-publication checks.

The intended audience is technical leaders who are not necessarily AI or ML specialists. Keep the writing calm, specific, and evidence-led. Preserve Sagar's voice, never invent facts or experiences, and do not use hype, excitable language, generic AI prose, or corporate filler.

## Medium migration

Existing Medium articles should be recreated as Markdown on `sagsan.com`, retain their original publication dates, and receive a light technical and editorial review. Do not add boilerplate announcing that an article was previously published on Medium; link to Medium only when the link is useful to the reader.

New writing should originate on `sagsan.com`. It may be syndicated to Medium, but `sagsan.com` should remain the canonical source. Changing canonical links or publishing on Medium is an external account action and requires user permission.

## Site and design principles

- Use Hugo with PaperMod and repository-local overrides; do not modify vendored theme files directly.
- Preserve PaperMod's plain, minimal, text-first aesthetic.
- Use PaperMod's standard Home Info Mode and chronological post cards rather than Profile Mode.
- Do not introduce custom colors, fonts, oversized typography, card systems, slogans, or broad visual overrides unless the user explicitly requests them.
- Keep primary navigation to Posts unless the content clearly requires more. The site title serves as the Home link.
- Use a custom minimal footer rather than the default PaperMod attribution footer.
- Prefer semantic HTML, accessible navigation, readable typography, and minimal JavaScript.
- Keep pages fast and avoid unnecessary dependencies, analytics, trackers, and third-party embeds.
- Use diagrams and images as evidence or explanation, not decoration.
- Store site content as plain Markdown where practical.
- Never commit credentials, private email configuration, API keys, or other secrets.
- Keep `README.md` and `docs/roadmap.md` current when implementation or editorial decisions change.

## Current implementation

- PaperMod is pinned as the `themes/PaperMod` Git submodule.
- The homepage uses PaperMod's standard Home Info Mode and post list configured in `hugo.toml`.
- No custom palette or font layer is currently used; preserve PaperMod's defaults.
- The custom footer lives in `layouts/_partials/extend_footer.html`.
- Compatibility overrides for the installed Hugo version live under `layouts/`; keep them minimal and compare them when upgrading PaperMod.
- All seven original Medium articles have been migrated to page bundles under `content/writing/`, with their images stored beside the Markdown.

## Change discipline

- Inspect the working tree before editing and preserve unrelated user changes.
- Make focused changes that match the current request.
- Run the most relevant available checks before handing work back.
- Do not rewrite Git history or perform destructive operations unless the user explicitly requests them.
- Do not commit changes unless the user asks for a commit.

## Expected commands

After the Hugo project is scaffolded, use these commands unless the repository defines alternatives:

```sh
hugo server -D
hugo --minify
```

The production build must succeed without warnings before deployment.
