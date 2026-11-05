---
title: "How I prepared for Google Cloud’s Professional Agentic Architect exam, and what I would do differently"
date: 2026-11-05T00:00:00-08:00
description: "How I prepared for the Professional Agentic Architect exam, the gaps quizzes exposed, and why I would spend more time configuring and evaluating agents."
summary: "How I prepared for the Professional Agentic Architect exam, the gaps quizzes exposed, and why I would spend more time configuring and evaluating agents."
tags: ["google-cloud", "ai-agents", "certification", "paa"]
draft: true
showToc: false
---

![Google Cloud Professional Agentic Architect certification, issued November 5, 2026](assets/agentic-architect-certification.png)

I’ve earned the Google Cloud Professional Agentic Architect certification. I spent about three weeks preparing for the written exam, but there is a fairly big asterisk next to that: I already had experience building agents on GCP and had passed the Professional Machine Learning Engineer exam.

That helped a lot. If you are starting from zero, my rough recommendation would be to allow three or four months to learn and actually build things.

I took the beta written exam on September 30, 2026. The certification also requires separate hands-on labs, which I have since completed. Check Google’s [certification page and FAQs](https://cloud.google.com/learn/certification/agentic-architect) for the current process, especially if you are taking it after the beta.

The most useful thing I wrote down after the written exam was: “Honestly I think I was missing depth.”

I felt reasonably good about how it had gone. I also knew there were things I could discuss quite comfortably until someone asked me to choose the actual configuration. Then it got less comfortable.

A lot of my preparation involved quizzes and working through the answers with AI. This helped, but it also taught me to be a little suspicious of a good quiz score.

Sometimes I knew the answer. Sometimes I recognized the product name. Sometimes three options were bad enough that the fourth one won by default. All three can look exactly the same on a results screen.

The useful part was explaining why I had picked an answer, and then being asked to apply the same idea in a different situation. That was usually where the gaps appeared.

Two-legged versus three-legged OAuth was one of mine. I had trouble with it, and OpenID Connect was another topic I was underprepared for. “Use OAuth” sounds like a reasonable answer until you have to explain whose authority the agent is using.

For external tools, Google documents [two-legged OAuth using client credentials](https://docs.cloud.google.com/iam/docs/auth-with-2lo-v2) for the agent’s own authority, and [three-legged OAuth](https://docs.cloud.google.com/iam/docs/auth-with-3lo-v2) for access on behalf of a specific user. The destination still has to enforce what that caller can access. I would spend more time configuring these flows if I were preparing again.

Evaluation was another area that needed several passes. At one point in practice I learned when a model-based evaluator was useful, then promptly tried to use one for something we could check directly: which tools were called, in what order, and with whose identity. A little knowledge, enthusiastically applied :).

Here is an invented practice example of another distinction I had to work through:

> An assistant retrieves last year’s policy and accurately repeats it. The policy has since changed. Can the answer be grounded and still give the user the wrong information?

Yes. The [groundedness check](https://docs.cloud.google.com/gemini-enterprise-agent-platform/reference/models/evaluation) compares the response with the context supplied to it. If that context is the old passage, the answer can be supported by it while still being wrong about the current policy. You also need to check correctness and whether retrieval selected the right version.

I needed more than one explanation of this. Getting the multiple-choice answer right once did not mean I could explain it properly the next day.

Other areas that caught me out were Agent Studio routes and internal settings, and choosing how to conduct evaluations in more complicated scenarios. My preparation had sometimes stopped at knowing what a service did. I needed to go further into using it.

This is also why I wanted a better way to practice.

I had already built [Quiz Trail](https://quiz-trail.web.app/#quiz) for PMLE preparation. After the Agentic Architect written exam, I put together a separate bank of **150 original PAA practice questions**, with AI assistance, using my study notes, public exam objectives, documentation and the topics I needed to understand better.

It is the resource I wish I’d had when I started preparing.

Each question has an explanation and a documentation reference. The bank covers all five exam domains, with extra practice around areas such as evaluation, identity and retrieval. These are independent practice questions, not official Google questions or copies of questions from my exam.

Here is a sample of what to expect from the practice bank. A sandboxed coding agent needs a package mirror and a staging API, but no arbitrary internet access. The question is how to limit data leaving the environment while still letting the agent do its work.

![Quiz Trail question PAA-0026 asks which policy best limits data exfiltration for a sandboxed coding agent, with four answer choices](assets/quiz-trail-paa-sample-question.png)

**Correct answer: D. Restrict egress to approved destinations and use narrowly scoped staging credentials.**

Filesystem, network, and credential controls address different paths. Restricting outbound network traffic (egress) to the destinations the task needs, and limiting what its credentials authorize, provides defense in depth. Filesystem isolation alone does not stop network leaks, a model’s explanation is not a network policy, and snapshots containing real credentials introduce new risk.

Getting D right is a useful start. The next step is explaining how I would configure those restrictions and test whether they hold.

The feature I still like most in Quiz Trail is the **Copy AI prompt** button. It copies the question and answer choices into a prompt that I can take to my preferred LLM. Small feature, but copying four options separately gets old quite quickly.

The follow-up I find useful is something like this:

> I chose B because of these constraints. Explain where my reasoning breaks down. Why is the next-best option weaker? Then give me a different scenario testing the same concept and wait for my answer.

That last part matters. If I can only answer the original wording, I probably need another round. I also want to revisit questions I guessed correctly, because the green tick is being a bit generous there.

AI makes this kind of back-and-forth easy. It also makes it easy to generate a convincing explanation for a weak question. We had to correct and retire some of our earlier practice items. I would keep the official docs open and challenge the answer when something does not add up, including on my own site.

As of November 5, **[ten PAA sample questions are public](https://quiz-trail.web.app/agentic-ai/sample-questions/)**. The full interactive PAA bank is in private preview and is marked coming soon. PMLE practice is already available. The public PAA samples let you see the questions and explanations without signing in.

Free, useful I hope, and yes, a shameless self-plug.

If I were doing the preparation again, I would use quizzes to decide what to build next. After struggling with tool authentication, I would configure a small example and follow the identity through the request. For evaluation, I would run a few cases, deliberately break something, and see which check catches it. I specifically came away wishing I had spent more time setting up a Gateway.

For resources, start with the [official exam guide](https://services.google.com/fh/files/misc/professional_agentic_architect_exam_guide_english.pdf) and the [Google Skills learning path](https://www.skills.google/paths/4525). Use the product docs to go deeper where practice exposes a gap. Google describes the learning path as a starting point, and that matches my experience of needing more depth than broad topic coverage alone.

If you try the [PAA samples on Quiz Trail](https://quiz-trail.web.app/agentic-ai/sample-questions/), spend some time on the explanation even when you get the answer right. Pick the alternative that almost convinced you and explain why it loses. I found those conversations much more useful than finishing another batch of questions.

Hope it helps, and good luck.
