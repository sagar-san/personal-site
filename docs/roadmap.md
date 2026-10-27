# Roadmap

This roadmap tracks the launch of the personal technical blog and public AI lab notebook at [sagsan.com](https://sagsan.com).

## Strategy

- **Domain:** `sagsan.com` — short, memorable, and professional.
- **Email:** `sagar@sagsan.com`, forwarded to the owner's Gmail inbox.
- **Purpose:** Document personal AI, ML, and data experiments while demonstrating the technical and program judgment expected of an exceptional AI TPM.
- **Direction:** A simple, distraction-free, writing-first indie-web aesthetic—not a conventional résumé or portfolio.
- **Stack:** Hugo, PaperMod with restrained local overrides, and GitHub Pages.
- **Ongoing cost:** Approximately $11 per year for the domain. Hosting, SSL, and software are expected to remain free.

## Phase 1: Domain and email — complete

- [x] Purchase `sagsan.com` through Porkbun.
- [x] Configure Porkbun email forwarding for `sagar@sagsan.com`.
- [x] Forward mail to the owner's Gmail account.
- [x] Test delivery from another email account.

## Phase 2: Build the site

- [x] Install Hugo locally.
- [x] Scaffold the Hugo site in this repository.
- [x] Select PaperMod as the site theme.
- [x] Add PaperMod without modifying vendored theme files.
- [x] Configure the site title, canonical URL, navigation, metadata, and theme.
- [x] Configure PaperMod's standard Home Info Mode and chronological article list.
- [x] Put email, LinkedIn, and Kaggle first in homepage social links and tighten the introduction spacing.
- [x] Add compact article-list images for the stock trader, golf Parts 1 and 2, Fashion-MNIST classification, Snoop-Speare, and the small-business chatbot.
- [x] Add the Posts section.
- [x] Add a minimal custom footer.
- [x] Configure tags, series, RSS, and article metadata.
- [x] Recreate the image-classification Medium article as Markdown with its original publication date and image.
- [x] Review the image-classification article for technical consistency, clarity, links, and reproducibility.
- [x] Recreate and review the remaining six Medium articles.
- [x] Document the audience, voice, evidence, and review workflow in a repository-local article review skill.
- [ ] Establish PMLE, stock-trader, and golf-analysis series when their first articles are ready.
- [x] Preview the site locally with `hugo server -D`.
- [ ] Check mobile and desktop layouts, keyboard navigation, links, and contrast.
- [x] Run a clean production build with `hugo --minify`.

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

## Publication-ready articles

- Transfer Learning I (July 1, 2026) and II (July 28, 2026): publication dates backdated at the author’s request, titles aligned, cross-links preserved, and draft flags removed locally. Part I reports 91.78% accuracy from its 10,000-image confusion matrix; Part II reports 91.1% F1 on the author-confirmed shared 600-image evaluation. Deployment is pending.

## Published project retrospectives

- Autonomous stock trader: published October 25, 2026, using the July 6 result of a 13% loss. Includes the demo and historical trade log.

## Possible follow-ups

- Golf swing analysis Parts 1 and 2 use publication dates September 1 and September 15, 2026, with embedded YouTube demos. Part 3 remains a draft dated October 15, 2026, with application screenshots.

- [ ] Add an RSS feed and make it easy to discover.
- [ ] Add a simple `robots.txt` and sitemap.
- [ ] Add social sharing metadata and a small site icon.
- [ ] Document the publishing workflow.
- [ ] Add lightweight build checks for links and generated output.
