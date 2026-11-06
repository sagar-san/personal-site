---
title: "Passing Google Cloud’s Professional Agentic Architect exam"
date: 2026-11-05T00:00:00-08:00
description: "Preparing for Google Cloud’s Professional Agentic Architect exam with Google Skills, AI-generated practice questions, and hands-on work."
summary: "Preparing for Google Cloud’s Professional Agentic Architect exam with Google Skills, AI-generated practice questions, and hands-on work."
tags: ["google-cloud", "ai-agents", "certification", "paa"]
draft: false
showToc: false
---

![Google Cloud Professional Agentic Architect certification, issued November 5, 2026](assets/agentic-architect-certification.png)

I earned the new Google Cloud Professional Agentic Architect certification in November 2026. I spent about three weeks preparing for the written exam. That was easier for me because I already had experience building agents with Google’s Agent Development Kit (ADK) on GCP and had passed the [Professional Machine Learning Engineer (PMLE) exam]({{< relref "/writing/passing-google-pmle" >}}).

If you already have cloud experience but are new to AI agents, my rough estimate would be two to three months of preparation.

I took the beta written exam on September 30, 2026. The certification also requires separate hands-on labs, which I have since completed. Check Google’s [certification page and FAQs](https://cloud.google.com/learn/certification/agentic-architect) for the current process.

## How I prepared

My preparation had two parts: completing the [Google Skills learning path](https://www.skills.google/paths/4525) and working through AI-generated practice questions. I used the [official exam guide](https://services.google.com/fh/files/misc/professional_agentic_architect_exam_guide_english.pdf) to check topic coverage and the product documentation to go deeper where practice exposed a gap.

Passing PMLE had already made the question format familiar. For PAA, I worked through questions and their answers with AI, using explanations and follow-up scenarios to check my understanding.

Several topics needed a refresher, and the easiest way to learn was to use the services in GCP. Two-legged versus three-legged OAuth was one of those topics, and OpenID Connect was another area I was underprepared for.

Evaluation also needed several passes. At one point in practice I learned when a model-based evaluator was useful, then promptly tried to use one for something we could check directly: which tools were called, in what order, and with whose identity.

Here is an original practice question:

> An assistant retrieves last year’s policy and accurately repeats it. The policy has since changed. Can the answer be grounded and still give the user the wrong information?

**Answer: Yes.** The [groundedness check](https://docs.cloud.google.com/gemini-enterprise-agent-platform/reference/models/evaluation) compares the response with the context supplied to it. If that context is the old passage, the answer can be supported by it while still being wrong about the current policy. You also need to check correctness and whether retrieval selected the right version.

## What I would do differently

If I were doing the preparation again, I would use quizzes to decide what to build next:

- For authentication and authorization, work through a small tool-access example in the Google Cloud console and follow the identity and permissions through the request.
- For evaluation, run a few cases, deliberately break something, and see which check catches it.
- Spend more time configuring Agent Gateway.

## Practicing with Quiz Trail

The exam was new, and I struggled to find suitable question banks. I had already built [Quiz Trail](https://quiz-trail.web.app/) for PMLE preparation. After the Agentic Architect written exam, I put together **150 original PAA practice questions** using my study notes, public exam objectives, documentation, and the gaps the exam exposed. These are independent practice questions, not copies of exam questions.

Here is a sample from the bank about limiting a coding agent’s access while still letting it do its work:

![Quiz Trail question PAA-0026 asks which policy best limits data exfiltration for a sandboxed coding agent, with four answer choices](assets/quiz-trail-paa-sample-question.png)

**Correct answer: D. Restrict egress to approved destinations and use narrowly scoped staging credentials.**

Filesystem, network, and credential controls address different paths. Restricting outbound network traffic (egress) to the destinations the task needs, and limiting what its credentials authorize, provides defense in depth. Filesystem isolation alone does not stop network leaks, a model’s explanation is not a network policy, and snapshots containing real credentials introduce new risk.

The question bank is free on [Quiz Trail](https://quiz-trail.web.app/). If you find it useful, consider [buying me a coffee](https://buymeacoffee.com/okeanos).

Hope it helps, and good luck.
