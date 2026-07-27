# Roadmap

This roadmap tracks the launch of the personal portfolio and blog at [sagsan.com](https://sagsan.com).

## Strategy

- **Domain:** `sagsan.com` — short, memorable, and professional.
- **Email:** `sagar@sagsan.com`, forwarded to the owner's Gmail inbox.
- **Direction:** A raw, distraction-free, text-first indie-web aesthetic.
- **Stack:** Hugo, Hugo Bear Blog, and GitHub Pages.
- **Ongoing cost:** Approximately $11 per year for the domain. Hosting, SSL, and software are expected to remain free.

## Phase 1: Domain and email — complete

- [x] Purchase `sagsan.com` through Porkbun.
- [x] Configure Porkbun email forwarding for `sagar@sagsan.com`.
- [x] Forward mail to the owner's Gmail account.
- [x] Test delivery from another email account.

## Phase 2: Build the site

- [ ] Install Hugo locally.
- [ ] Scaffold the Hugo site in this repository.
- [ ] Add the Hugo Bear Blog theme by Jan Raasch.
- [ ] Configure the site title, canonical URL, navigation, metadata, and theme.
- [ ] Create a custom bio and résumé landing page.
- [ ] Add selected project pages.
- [ ] Write the first blog posts as Markdown files.
- [ ] Preview the site locally with `hugo server -D`.
- [ ] Check mobile and desktop layouts, keyboard navigation, links, and contrast.
- [ ] Run a clean production build with `hugo --minify`.

## Phase 3: Publish with GitHub Pages

- [ ] Create or select a public GitHub repository for the site.
- [ ] Push the Hugo source to GitHub.
- [ ] Add and verify a GitHub Actions workflow for Hugo Pages deployment.
- [ ] Enable GitHub Pages with GitHub Actions as the source.
- [ ] Set `sagsan.com` as the custom domain in GitHub Pages.
- [ ] Add the required GitHub Pages records in Porkbun DNS.
- [ ] Confirm that DNS resolves to GitHub Pages.
- [ ] Wait for GitHub to issue the TLS certificate and enforce HTTPS.
- [ ] Verify the live site, canonical links, custom domain, and error page.

## Possible follow-ups

- [ ] Add an RSS feed and make it easy to discover.
- [ ] Add a simple `robots.txt` and sitemap.
- [ ] Add social sharing metadata and a small site icon.
- [ ] Document the writing and publishing workflow.
- [ ] Add lightweight build checks for links and generated output.
