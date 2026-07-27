# AGENTS.md

## Purpose

This repository contains the source for Sagar's personal website at `sagsan.com`. The intended stack is Hugo with the Hugo Bear Blog theme, deployed through GitHub Pages.

## Working agreement

Agents are authorized to inspect, create, edit, move, and delete files within this repository when doing so is necessary to complete the user's request. They may also run local development tools, formatters, tests, builds, and Git commands whose effects remain within this repository.

Ask for the user's permission before any action that affects resources outside this repository, including:

- creating, changing, or deleting remote services or cloud resources;
- purchasing or configuring domains, DNS, email forwarding, or paid services;
- installing system-wide software or changing machine-level configuration;
- reading from or writing to files outside the repository;
- publishing, deploying, pushing, opening pull requests, or otherwise changing remote repositories;
- sending messages or acting as the user with an external account.

An explicit user request for one of those actions counts as permission only for the stated action and scope. Follow platform approval requirements even when this file grants repository-local authority.

## Project principles

- Preserve the raw, distraction-free, text-first indie-web aesthetic.
- Prefer semantic HTML, accessible navigation, readable typography, and minimal JavaScript.
- Keep pages fast and avoid unnecessary dependencies, analytics, trackers, and third-party embeds.
- Store site content as plain Markdown where practical.
- Never commit credentials, private email configuration, API keys, or other secrets.
- Keep documentation and `docs/roadmap.md` current when implementation decisions change.

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
