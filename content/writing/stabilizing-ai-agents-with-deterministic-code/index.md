---
title: "How to Stabilize an AI Agent That Keeps Changing Its Mind"
date: 2026-07-27T00:00:00-07:00
description: "Using explicit state transitions, fail-closed behavior, and deterministic authorization around a probabilistic trading agent."
summary: "Using explicit state transitions, fail-closed behavior, and deterministic authorization around a probabilistic trading agent."
tags: ["ai-agents", "ml-systems", "agent-safety", "algorithmic-trading"]
series: ["autonomous-stock-trader"]
draft: true
showToc: true
TocOpen: false
---
<!-- EDITORIAL FEEDBACK

Overall assessment: This is the stronger standalone argument of the pair. It has a clear failure, three concrete iterations, an enforceable design boundary, and an appropriately narrow conclusion. The thermostat explanation makes hysteresis accessible without oversimplifying it.

Keep it separate from the system overview. Its lesson applies beyond trading—to payments, infrastructure, publishing, and messaging—while the first article needs room for the architecture, strategy, results, and dashboard. Publish it second and cross-link it to the overview near the opening and conclusion.

Before publication:

1. Add the required statement that this documents a personal engineering project and is not financial advice.
2. Replace both visual placeholders and ensure diagrams distinguish recommendation, authorization, and execution precisely.
3. Verify the July 20 recommendations and timestamps, the 21-day rule, the five-round limit, every listed policy control, and the “99 tests passed; two integration tests failed” result against durable records. Explain the test scope briefly enough that readers do not mistake unit coverage for system validation.
4. Define what “stable enough that I could trust the process” means or narrow the claim. The article later acknowledges that stability and strategy quality remain unproven, but the opening currently sounds more conclusive.
5. Consider narrowing the title during the edit pass. “How to Stabilize an AI Agent” reads as a general prescription, while the evidence comes from one system and a limited verification run.
6. Confirm that the GitHub and dashboard URLs are final and safe for public use.

The article is structurally sound, but its operational claims still need source verification before publication.

END EDITORIAL FEEDBACK -->

# How to Stabilize an AI Agent That Keeps Changing Its Mind

## What a jittery stock trader taught me about using deterministic code around probabilistic systems

> **Visual placeholder — hero image:** A restrained three-layer diagram labeled “Propose,” “Critique,” and “Authorize,” with the final layer shown as deterministic code rather than another AI agent.

At 7:00 a.m. on July 20, my autonomous stock trader recommended holding MU, MRVL, and SNDK. At 10:00 a.m., it reached the same conclusion.

At 1:00 p.m., it proposed liquidating MU and MRVL.

The portfolio had not undergone some fundamental transformation in those six hours. The agents had interpreted the rules differently. That was the latest version of a problem I had been trying to fix for weeks: every run began a little too much like a new conversation.

This matters in any agentic system, but especially when the output can move real money. A recommendation that sounds plausible is not the same thing as a decision you can safely execute.

It took three attempts to make the system stable enough that I could trust the process, even if I still do not know whether its investment strategy is any good.

## Attempt one: add a critic

The original trader used one portfolio agent. It received market data, news sentiment, technical indicators, and current holdings, then proposed an allocation. Run it again with slightly different inputs and it might prefer a different stock.

The agent was not exactly behaving irrationally. It simply had no durable conviction linking one run to the next, and language models are very good at making the latest answer sound complete.

My first fix was to add a senior risk advisor. The portfolio analyst makes a proposal; the advisor checks position limits, entry conditions, cash, and holding periods. If it rejects the plan, the analyst revises it. They can repeat that loop up to five times.

The recommendations became more deliberate. Still, asking a second probabilistic model to check the first one is not a hard safety boundary. The critic can omit a check, misread a number, or approve a bad interpretation. More reasoning helped, enforcement was still missing.

## Attempt two: make decisions harder to reverse

Next I added a 21-day minimum holding period. This was a practical anti-jitter rule based on observing volatile AI stocks such as Micron. During the previous six months, the daily price could swing sharply while the direction across two or three weeks was often more consistent.

In control-system terms, this is hysteresis: entering a state and leaving it use different thresholds. A thermostat does this so it does not switch on and off every few seconds around one temperature. Agents need similar friction when a fresh output would otherwise overwrite the previous decision.

The holding period reduced churn, but the agents still interpreted when the rule applied. That is what failed on July 20. The later run used a sentiment-volatility limit intended for *new purchases* as a reason to sell positions already in the account.

The rule itself made sense, the context was wrong. “I would not buy this stock today” does not mean “I should sell the stock I already own.” Written down, that seems obvious. It was not obvious inside a long prompt containing many overlapping rules.

## Attempt three: move authority out of the prompt

The durable fix was to stop treating the agents’ conclusion as authorization.

The portfolio analyst and risk advisor still handle work that benefits from a language model: interpreting news, comparing imperfect options, explaining trade-offs, and proposing a target portfolio. Their output is structured data rather than a broker command.

An ordinary Python policy engine then rebuilds the decision and checks it independently. It requires a final advisor approval, the correct account, allowed tickers, fresh prices, sufficient cash, valid target weights, no more than three positions, and a unique decision ID. If a required input is missing, stale, or contradictory, the run is cancelled. Confidence has no bearing on the result.

The policy also labels every proposed change as one of five actions: enter, add, hold, reduce, or exit. Those actions have different rules. High sentiment volatility may prevent a new entry, for example, but it cannot create a reason to exit an existing position. Early exits require separate evidence such as severely negative sentiment, a trailing-stop breach, or a broad market risk-off signal.

This distinction applies well beyond trading. The reason an agent may create something is not necessarily a valid reason to modify or delete it. Payments, infrastructure changes, published content, and customer messages all have lifecycles. Modeling those state transitions explicitly is much safer than giving a model one vague instruction to “proceed if appropriate.”

> **Visual placeholder — authorization boundary:** Show the analyst and critic above a thick line. Below it, show structured proposal → Python policy → broker executor. Label the line “No natural-language authority beyond this point.”

## Failure should remove authority, not invent state

The broker integration exposed a second problem. Early helpers could fall back to an empty portfolio and $100 in cash when an account request failed. Convenient during development, dangerous in production. An authentication or parsing error could look exactly like a fully funded account.

The system now fails closed. Cash, buying power, holdings, and prices must come from valid broker responses. A missing quote does not become zero. An unreadable portfolio does not become empty. If the result of one order is uncertain, the rest of the sequence stops until the actual broker state can be reconciled.

The rule I use now is simple: uncertainty should remove authority. It should not create a safe-looking default.

Time is part of the policy as well. If a portfolio needs to sell one stock and buy another, the system only sells on that market date. Any replacement purchase requires a fresh run on a later date with current prices, holdings, cash, and risk data. That prevents a failed sale from being followed by a buy using money the account may not have.

Two deterministic controls can override the normal 21-day hold. A trailing stop follows a position upward but never moves down. A broad-market circuit breaker checks whether SPY is below its 200-session average. Either can force defensive action without asking the agents to reinterpret the situation.

## What changed

The final architecture has four jobs. One agent proposes. A second criticizes. Deterministic policy decides whether the state change is legal. Deterministic execution performs only the authorized operation.

The safety overhaul added strict broker parsing, stable decision IDs, action-specific rules, trailing stops, a market circuit breaker, a kill switch, and structured audit records. In a local verification run, 99 tests passed; two network-backed integration tests failed because the restricted environment could not reach Google services.

That test result supports the code contracts. It does not prove the trader will make money, that Gemini’s sentiment scores are consistent, or that the agent will never find a new way to be jittery. Those need backtesting, evaluation, and time.

The broader lesson is fairly narrow: use agents for interpretation and proposals, then put deterministic code at the point where a suggestion becomes a consequential action. Do not ask the language model to be its own control system.

The complete implementation is available on [GitHub](https://github.com/Ameenota/stock-trader), and the live [portfolio dashboard](https://portfolio-dashboard-412197301452.us-central1.run.app) shows the results, including the losses. Stability does not make a strategy correct. It does make the strategy possible to test, which has to come first.
