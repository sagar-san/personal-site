---
name: article-review
description: Draft, edit, or review articles for sagsan.com. Use for any article work, including structural reviews, technical reviews, copyedits, rewrites, migration reviews, and pre-publication checks of Markdown under content/writing/.
---

# Article Review

Apply these standards whenever drafting, editing, or reviewing an article. Preserve Sagar's conversational voice while making the argument easier to follow and the technical claims easier to inspect.

## Write for the audience

Write for technically literate leaders who understand software, data, systems, products, and delivery but may not have specialist AI or machine-learning knowledge.

- Explain an AI/ML term when a general technology leader may not know it, then use the precise term consistently.
- Supply enough context to understand why a technical choice matters. Do not turn the article into a beginner tutorial.
- Connect implementation detail to an outcome, constraint, decision, risk, or tradeoff.
- Let engineers inspect the reasoning without requiring every reader to follow every implementation detail.

## Serve two reading depths

For a substantial article:

1. Open with a concise account of the problem, why it matters, what was built, the result, and the most important decision. Do not delay the point with scene-setting.
2. Follow with enough architecture, implementation, evaluation, tradeoff, and failure detail for a technical reader to inspect the work.

Include the following when the material supports them:

- the question or hypothesis;
- success criteria and primary metrics;
- scope and constraints;
- architecture and important dependencies;
- alternatives considered and decisions made;
- implementation details and reproducible artifacts;
- results clearly separated from interpretation;
- limitations, risks, and failure modes;
- what changed during the work;
- next steps or what production use would require.

Do not force every article into the same template. Short notes and narrowly scoped experiments may need only the relevant elements.

## Use a restrained voice

Keep the prose calm, direct, specific, and evidence-led. Do not use excitable language.

- Avoid hype such as "groundbreaking," "game-changing," "revolutionary," "powerful," "exciting," "impressive," or claims that a tool "unlocks" or "transforms" something unless the evidence genuinely requires that wording.
- Avoid generic AI-written openings and transitions such as "in today's rapidly evolving landscape," "let's dive in," and "this is where X comes in."
- Avoid inflated contrast formulas such as "not just X, but Y," repeated rhetorical questions, unnecessary three-part lists, excessive em dashes, and conclusions that merely restate the introduction.
- Do not call a result successful, robust, scalable, production-ready, or significant without defining the evidence behind the claim.
- Prefer concrete nouns, active verbs, measured claims, and varied sentence structure.
- Use first person where it reflects Sagar's actual work or judgment. Do not manufacture personal experience.
- Do not add slogans, sales copy, thought-leadership clichés, or enthusiasm as a substitute for substance.

Treat "AI slop" as a review failure: prose that is polished but generic, repetitive, over-signposted, inflated, vague about evidence, or detached from the author's actual decisions. Rewrite it with concrete project details or remove it.

## Review evidence and technical claims

- Prefer specific evidence over generic claims.
- Never invent a result, metric, artifact, decision, quotation, or personal experience.
- Mark estimates, assumptions, and inference clearly.
- Reconcile conflicting numbers, dates, units, terminology, and descriptions before publication.
- Separate observed results from the author's interpretation of them.
- Link code, notebooks, datasets, demos, diagrams, or result plots only when they materially support the article.
- Flag unverifiable or unsupported claims for Sagar instead of quietly strengthening or completing them.
- Check that conclusions stay within the scope of the evidence and that limitations are stated plainly.

For financial experiments, state clearly that the article documents a personal project and is not financial advice. For every project, remove secrets, credentials, private infrastructure details, personal data, and sensitive operational information.

## Follow the review workflow

1. Read the complete article before changing individual lines. Inspect linked repository artifacts when they are necessary to assess a claim.
2. Identify the article's central question, result, and decision. If any is unclear, treat that as a structural issue rather than hiding it with smoother prose.
3. Check evidence, internal consistency, technical accuracy, scope, and missing limitations.
4. Check the two reading depths and reorder material when the main point arrives too late.
5. Edit for the intended audience and restrained voice. Preserve distinctive wording that sounds like Sagar unless clarity or accuracy requires a change.
6. Make the requested edits when the user asks for editing or rewriting. When the user asks only for a review, report prioritized findings without changing the article.
7. After edits, reread the article as a whole and report any claims or decisions that still require Sagar's confirmation.

Prioritize substantive findings over minor copyedits. Do not turn a personal technical article into corporate prose.
