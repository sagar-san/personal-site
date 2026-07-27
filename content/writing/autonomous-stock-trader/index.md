---
title: "I Built an Autonomous Stock Trader With AI Agents. Then I Gave It $100."
date: 2026-07-27T00:00:00-07:00
description: "Building an agent-based stock-trading system, connecting it to live execution, and examining what an early loss revealed about the design."
summary: "Building an agent-based stock-trading system, connecting it to live execution, and examining what an early loss revealed about the design."
tags: ["ai-agents", "ml-systems", "algorithmic-trading", "bigquery"]
series: ["autonomous-stock-trader"]
draft: true
showToc: true
TocOpen: false
---
<!-- EDITORIAL FEEDBACK

Overall assessment: This is in decent draft shape. It has a clear narrative, explains the system at an accessible technical level, reports an unfavorable result honestly, and identifies the agent/code authorization boundary as the main design decision. It works as the series overview.

Keeping this separate from the deterministic-controls article makes sense. This article answers “What did I build, how did it perform, and what did I learn?” The companion answers “How did I make consequential agent behavior more stable and enforceable?” Publish this one first and add explicit links in both directions.

Before publication:

1. Replace COURSE_LINK_HERE and every visual placeholder. Confirm that screenshots expose no account identifiers or private brokerage details.
2. Verify every date-bound result against the source records: the starting balance, $71.94 equity, 28% loss, $11.01 cash, $25.74 unrealized loss, eight live orders, holdings, and observation dates. State the exact benchmark window in the comparison chart.
3. Reconsider the headline and a few excitable lines, especially “woohoo,” “financial crisis,” and “victory lap.” They give the opening more Medium-style energy than the site’s restrained editorial standard calls for.
4. Clarify the evidence behind “tested the workflow enough” and “stable decisions” or narrow those statements. The article describes stronger controls but does not yet define a stability criterion.
5. Confirm that the public dashboard and GitHub URLs are final and that the GitHub account name is intentional.
6. Fix minor grammar during the edit pass, including “There was a lot of moving pieces” and the comma splice that follows it.

Do not publish until the figures and financial claims have been verified. Keep the existing personal-project/not-investment-advice disclaimer.

END EDITORIAL FEEDBACK -->

# I Built an Autonomous Stock Trader With AI Agents. Then I Gave It $100.

## It screens the market, reads the news, debates its own recommendations, places real Robinhood orders, and publishes the results

> **Visual placeholder — hero image:** Use a clean screenshot of the live dashboard showing the portfolio, current recommendations, and performance chart. Crop out any account identifier.

The first time my stock-trading agent placed a live order, I got two notifications. One came from the application through Discord. The other came from Robinhood.

I had tested the workflow enough to know what should happen, but seeing the order land in a real brokerage account was still a genuine “woohoo” moment. The project had crossed the line between producing an answer and actually doing something.

I built it for the [Kaggle five-day agents course](COURSE_LINK_HERE). My goal was not to make another chatbot or a polished demo that stopped at a recommendation. I wanted an end-to-end application: gather market data, decide what to buy, execute the trade, record it, and show the result in a public dashboard.

The test account started with roughly $100. Enough to make the decisions real, but not enough to turn an experiment into a financial crisis.

As of July 20, 2026, the account was worth $71.94.

So this is not a victory lap. It is a walkthrough of what I built, how it works, and what a 28% loss taught me about putting AI agents in charge of consequential actions.

*This is an engineering experiment, not investment advice.*

## I built it one small piece at a time

The first end-to-end version took a weekend to wire up. Testing, debugging, and stabilizing it took another week or two.

I wrote very little of the code myself. Antigravity and Codex produced most of the implementation, which I think is increasingly how software will get built. My work was closer to product development: define the architecture, break the product into features, inspect the behavior, and tighten the requirements when reality disagreed with the plan.

I did not ask an agent to “build me a stock trader.” First, I asked it to connect to Robinhood. Then to write and query BigQuery tables. Market-data ingestion came next, followed by sentiment analysis, portfolio decisions, execution, and the dashboard. Even the Streamlit visualizations were built and checked separately before I brought them together.

There was a lot of moving pieces. The code came together fast, figuring out whether the full system was safe and coherent took much longer.

> **Visual placeholder — architecture:** A simple left-to-right diagram: market data and news → stock screener → Gemini sentiment → analyst/risk debate → deterministic policy → Robinhood → BigQuery → Streamlit dashboard.

## The financial strategy, in plain English

The trader focuses on volatile companies connected to AI infrastructure. Its current allowlist has 42 symbols: semiconductor companies, cloud platforms, data-center suppliers, power companies, software and security firms, plus one defensive Treasury ETF. These are risky holdings on purpose. A stock in this group can move 10% in a day.

Each run starts with cheap, deterministic screening. Python narrows the 42-symbol universe to an 11-stock watchlist using price history. Anything already owned stays on the list. Other stocks generally need to trade above their 50-day average, although a deeply oversold company can qualify as a possible value opportunity. This stage uses no language-model tokens.

Gemini then reads recent news for the shortlist and scores the tone from -1 to +1. The pipeline blends that result into a five-day weighted average, so one dramatic headline does not completely replace the previous view.

The system looks for two kinds of entry. A “dip” candidate is at least 10% below its one-year high but still has positive, reasonably stable news sentiment. A “breakout” candidate is reaching a recent high while its price momentum turns upward. Put simply: one path looks for a good company on sale, the other looks for a strong company getting stronger.

The portfolio can hold no more than three positions, usually targeting about 30% in each and leaving the rest in cash. I later added a 21-day minimum hold because the early recommendations changed far too often. The number was a hypothesis based on watching stocks such as Micron: daily prices could be chaotic, while the trend over two or three weeks had often been more consistent during the previous six months. The market stopped cooperating with that pattern almost as soon as I launched, which is useful context.

## AI recommends; Python decides

One Gemini agent acts as a portfolio analyst and proposes target allocations. A second acts as a senior risk advisor. It checks the proposal and can send it back for revision, up to five rounds.

I added the second agent because the first one kept changing its mind between runs. The debate helped, but it did not solve the underlying problem. A second language model can misunderstand a rule just as easily as the first.

The current system therefore treats the agents’ output as a recommendation. A deterministic Python policy independently checks the account, ticker allowlist, prices, cash, position limits, holding rules, and final approval before an order can reach Robinhood. The broker operation itself is also handled by regular Python rather than free-form agent behavior.

That boundary—agents analyze, code authorizes—ended up being the most important design decision in the project. The companion article explains why it took several attempts to get there.

## The live results are not good

BigQuery records eight live orders between June 24 and July 1. The first portfolio bought MU, DELL, and TSM. Later runs reduced MU, sold DELL and TSM, and bought MRVL and SNDK. By July 20, the account held MU, MRVL, and SNDK.

Then the AI sector sold off. The July 20 snapshot showed $71.94 in total equity, including $11.01 in cash, with $25.74 of unrealized losses. Three ticker symbols did not provide much diversification when all three were versions of the same concentrated AI bet.

> **Visual placeholder — results:** Add the dashboard chart comparing the portfolio with the S&P 500 from launch through July 20, 2026. Caption it with the exact observation window and the roughly $100 starting balance.

It would be easy to blame bad timing. If the system had launched during an earlier Micron rally, the screenshot would look much better. But that would not prove the strategy worked, either. A few weeks, three current positions, and a tiny account cannot establish investment performance. What the loss does show is how quickly concentration and market regime can overwhelm a plausible trading thesis.

## The dashboard makes the experiment inspectable

Every run writes market metrics, recommendations, portfolio snapshots, and trade history to BigQuery. A separate Streamlit frontend turns those records into the public [portfolio dashboard](https://portfolio-dashboard-412197301452.us-central1.run.app).

The dashboard shows more than completed trades. It shows which stocks the system analyzed, the technical and sentiment measures behind its recommendations, the current portfolio, and performance against the S&P 500. Because it reads from BigQuery, it also remains separate from Robinhood authentication and does not need a live brokerage session every time somebody opens the page.

You can explore the [live dashboard](https://portfolio-dashboard-412197301452.us-central1.run.app) or read the [source code on GitHub](https://github.com/Ameenota/stock-trader).

## What I would do before adding more money

Right now, live execution is disabled while I watch the new safeguards in simulation. I want to see several days of stable decisions before considering another live run.

The larger gap is backtesting. The full pipeline needs to be replayed against historical data without giving it access to future information, while tracking a realistic simulated account and trading costs. The sentiment layer needs its own evaluation too. Gemini produces a score, but I have not yet proved that the score is consistent or useful.

Would I give the agent more than $100 today? Honestly, I am not sure. The system now has much stronger engineering controls, but the investment strategy is still unproven. Those are two different questions, and building this made the difference very clear.
