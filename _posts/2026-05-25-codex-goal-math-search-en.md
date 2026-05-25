---
layout: post
title: "Digging Through a Mathematical Search Space with Codex Goal: From 3.9 Billion Pairs to 180 Million Residuals"
date: 2026-05-25
lang: en
description: "A competition retrospective on goal-driven AI collaboration, Lean certificates, and large-scale solver engineering."
tags:
  - AI for Math
  - Codex
  - Lean
  - Formal Verification
  - Competition Engineering
---

Language: [中文]({% post_url 2026-05-25-codex-goal-math-search %}) / English

> Note: This is a public technical article written while the competition is still in progress. It preserves the methodology, collaboration experience, and aggregate scale, while intentionally omitting the current solver's predicates, proof templates, shape allowlists, strategy ordering, internal endpoints, prompts, script parameters, and unpublished data.

Recently, I have been participating in SAIR Foundation's [Mathematics Distillation Challenge: Equational Theories Stage 2](https://competition.sair.foundation/competitions/mathematics-distillation-challenge-equational-theories-stage2/overview).

The competition studies equational theories: implication problems between algebraic equations. Roughly speaking, given two equational laws `E1` and `E2`, the task is to decide whether `E1 => E2` holds. The key threshold in Stage 2 is that an answer cannot be merely true or false. It must come with a Lean 4-verifiable certificate. A true statement needs a proof, a false statement needs a counterexample, and the final result is accepted only when a deterministic Lean judge accepts the certificate.

That changes the nature of the problem.

Without the formal verification requirement, one might simply ask a large language model:

> Does this equational implication hold?

In Stage 2, however, a natural-language explanation, a plausible proof sketch, or a confident true / false judgment is not the final result. Only an accepted Lean certificate counts as solving the pair.

So the real question I have been working on is not simply:

> Can GPT answer this mathematical question?

It is closer to:

> With Lean certificates as the final boundary, can Codex become a long-running engineering partner for mathematical search?

This article is about that process: how I used Codex's `goal` feature to turn fragmented conversations into sustained exploration, and how that helped push a search space of roughly 3.9 billion pairs down to about 180 million hard residuals.

## What This Article Is Really About

This is not a post about revealing a magical competition solution. The competition is still ongoing, so I will not disclose the current solver's concrete rules, templates, strategy order, or internal verification resources.

What I want to describe is a way of working:

> An LLM does not have to become the mathematical judge. It can become the fast, patient, persistent collaborator sitting next to a formal mathematics system.

In this project, Codex's value has not mainly been in producing one final Lean proof in a single shot. Its value has been in continuously helping with steps such as:

- reading failed samples and statistical summaries;
- identifying structural patterns in the residual set;
- generating and refactoring analysis scripts;
- helping design deterministic strategies;
- maintaining solver versions and experiment notes;
- proposing repair directions from Lean / judge failures;
- turning scattered observations into a reproducible pipeline.

But every result still has to return to the Lean certificate boundary. That boundary is essential: the LLM can propose candidates, but it cannot replace the verifier.

## From Chat Assistant to Long-Running Collaborator

In ordinary chat, I might use AI like this:

> Help me inspect this log.<br>
> Help me write a statistics script.<br>
> What pattern do you see in this failed sample?<br>
> What should I try next?

All of that is useful, but each request feels like a separate task.

This competition is not a single-point problem. It is a long chain: read data, merge structures, mine patterns, write scripts, run verification, inspect failures, adjust strategies, record versions, and continue to the next round. If everything is handled as one-off prompts, context becomes fragmented, and it becomes easy to forget which experiment proved what.

Later, I began using Codex's `goal` feature to describe the work as a persistent objective: analyze the current residual set, look for new reduction opportunities, validate candidate strategies, record the result, and keep going.

The difference was obvious.

Codex was no longer just answering the latest question. It was working around the same objective over time. It would read existing statistics, compare solver versions, inspect strategy registries, write temporary analysis scripts, summarize failure causes, suggest the next experiment, and keep adjusting after validation results came back.

I still controlled the boundary and the judgment: what can go into the formal solver, what remains only a candidate, and what cannot be published during the competition. But a lot of intermediate progress no longer required me to manually decompose every step.

That produced a very particular kind of momentum.

It was not the feeling that AI had done mathematical research for me. It was more engineering-shaped: handing a huge, messy, almost unapproachable problem to a persistent collaborator, and watching it slowly become tables, scripts, certificates, logs, and a falling residual count.

## A Typical Goal Loop

Our loop often looked like this:

```text
set a goal
  -> read the current residual and coverage statistics
  -> find a large bucket or strategy gap
  -> write a script for aggregate analysis
  -> generate a candidate rule / finite-model check / proof-template idea
  -> run focused validation
  -> summarize hits, conflicts, and failure causes
  -> decide whether it deserves to enter the formal system
  -> look for the next opportunity
```

The loop itself is not mysterious. The important thing is that it can happen frequently.

In this collaboration mode, `3.9 billion pairs -> 180 million residuals` was not a single flash of insight. It was a sequence of small, verifiable steps: reducing duplicated structure today, confirming a false family tomorrow, repairing a proof template the day after, then discovering that a statistical accounting method had a bug.

Not every step was impressive. But every step could be written down, reproduced, and checked by the judge.

That feels completely different from asking an AI for an answer. It is more like working with a continuously available research assistant inside a mathematical search space. It does not decide the final mathematical truth for you, but it makes the observe, hypothesize, implement, verify, and record loop much faster.

## Why 3.9 Billion Is Hard

The original space contains roughly 3.9 billion pairs. That number is easy to turn into a slogan, but the real difficulty is not just that it is large. It is large enough that, without structured accounting, you cannot tell what you have actually done correctly.

At this scale, even basic questions become hard:

- How much new coverage did a strategy add?
- Is it only covering cases that other strategies had already solved?
- Is it solving a large bucket or only sparse long tail cases?
- Could it introduce true / false conflicts?
- Can its certificates be verified by Lean reliably?

So the first goal of the pipeline is not proof. It is making the space countable, comparable, and auditable.

At a high level, I think of the process as:

```text
raw algebraic pairs
  -> canonicalization
  -> bucket / hash / metadata
  -> deterministic filters
  -> finite-model counterexample search
  -> proof-template attempts
  -> Lean / judge verification
  -> residual analysis
```

The core principle is simple: anything entering the accepted set must have a certificate. Anything remaining in the residual set should, as much as possible, carry an explanation of why it has not yet been solved.

## From 3.9 Billion to 180 Million

After multiple rounds of canonicalization, deterministic filtering, finite-model search, and proof-template coverage, the original space of roughly 3.9 billion pairs was pushed down to about 180 million hard residuals.

The meaning of that result is not only that the count went down.

More importantly, the remaining 180 million cases are not a random unsolved set. They are a residual set that has survived a strong baseline. They have avoided a set of cheap rules, have not been quickly refuted by the current finite-model strategies, and have not fallen into existing proof templates.

In other words, they expose the current solver's real blind spots.

That is why I find residuals valuable. In large-scale formal search, it is tempting to focus only on solved counts. But the residual set is also an asset. It tells you where to dig next, which structures have not been explained, and which directions may deserve to become new benchmarks.

## Where Codex Helped

In this project, Codex's most important contribution was not producing one Lean proof in isolation.

It helped keep a huge, messy, failure-prone exploration process inside a rhythmic loop:

- quickly reading scripts, logs, and failed samples;
- turning temporary observations into repeatable statistics;
- generating candidate implementations and handing them back to the judge;
- comparing coverage deltas across solver versions;
- summarizing failure causes into the next strategy;
- preserving context and direction across long-running exploration.

The efficiency of that collaboration felt distinctive.

It was not "AI completed the mathematical research for me." It was "AI made the research engineering loop run faster." The final judgment still came from Lean certificates, but Codex shortened the distance from observation to experiment, from experiment to summary, and from summary to the next step.

My favorite moments were when a vague idea became a verifiable result through several small steps. First we would notice that a residual bucket looked unusually large. Then Codex would help pull samples, write statistics, summarize structure, generate candidate checkers, and organize the result into a decision about whether to keep going. Even when a strategy failed, it became a negative sample for the next search round instead of wasted effort.

That is where `goal` mode fits this kind of work especially well: it turns "help me do one thing" into "work with me in this direction."

## Why the Verifier Boundary Matters

One issue with LLMs is that they can produce answers that sound reasonable without being reliable. Formal mathematics amplifies that problem. A proof with one hidden assumption, or a Lean file with one dependency escape, can be rejected.

So I do not want Codex to be the judge.

I prefer placing it around the verifier:

- Codex proposes candidates;
- scripts make them batchable and reproducible;
- the solver generates certificates;
- the Lean judge accepts or rejects the final result;
- residual analysis explains failures and opens the next round.

This structure gives me a calm kind of speed. Codex can explore aggressively, but an accepted result cannot enter the system by confidence alone. It has to pass through the Lean certificate gate.

That is also what made this collaboration so compelling: exploration was fast, but the progress that remained was hard.

## What I Hope Readers Take Away

If you work on AI for math, formal methods, LLM agents, or large-scale search systems, I think several lessons generalize.

First, put the LLM around the verifier, not in the verifier's seat. The LLM can propose candidates, write code, and explain failures, but the acceptance criterion should come from Lean or another judge.

Second, make the problem space countable before trying to solve it. Without canonicalization, buckets, hashes, and residual accounting, coverage can easily become an illusion.

Third, treat failed samples as assets. A large residual set is not a trash pile; it is the entry point for the next round of strategy mining.

Fourth, persistent goals matter more than single prompts. The clearest effect of Codex's `goal` feature was that, when AI can keep working around a goal, it changes from a question-answering tool into an exploration partner.

Fifth, the excitement comes from verified progress. In competition engineering, the most satisfying moment is not when a model says "I think this is right." It is when the judge accepts, the statistics go down, the version record closes, and the next round can build on something solid.

## What Is Not Public Yet

Because the competition is still ongoing, this article intentionally omits everything that could directly reproduce the current solver, including:

- concrete predicates;
- proof templates;
- strategy order;
- shape lists;
- internal verification resources;
- prompts;
- script parameters;
- unpublished data files.

I want this article to make the method and experience public, not the competition answer.

After the competition, if conditions allow, I may turn the internal version into a more complete technical report: strategy categories, coverage statistics, a residual taxonomy, benchmark design, and a system-level comparison between different LLM and symbolic baselines.

## Closing

From roughly 3.9 billion pairs to about 180 million residuals, this is not just a compression number.

For me, it is a new collaboration experience: the human sets the objective and boundary, Codex keeps the exploration and engineering loop moving, and the Lean judge verifies the mathematical truth.

That is where the interesting part begins.

An LLM does not have to become the mathematical judge. It can become the fast, patient, persistent collaborator sitting next to a formal mathematics system.

And when every real step forward can be verified by a certificate, that collaboration is not only fast. It is solid.
