---
layout: post
title:  "When NOT to build a model"
date:   2026-09-26 22:45:00 +0800
categories: data-analytics ai
tags: [machine-learning, analytics, decision-framework, field-notes]
---

The fastest way to lose the plot on a data project is to start from the answer — *"let's build a model for that"* — before anyone has said clearly what decision it's supposed to improve. Machine learning has become the default ambition of far too many analytics requests, and the cost of that reflex is quietly enormous. Most problems asking for a model don't need one. A well-built dashboard, a plain business rule, or even a genuinely cleaned dataset answers them better, faster, and in a way people actually trust.

This is the filter I run before I let anyone (including me) greenlight modelling work. It's three questions, and a model has to survive all three to be worth building.

## Test 1 — Can you state the decision the model changes?

Every model exists to make a specific decision better, cheaper, or faster. If you can't name that decision in one sentence — *who* acts on the output, *what* they do differently, and *how* you'd know the world got better — you don't have a modelling problem yet. You have a reporting problem, or a "we're curious" problem. Both are legitimate, but neither deserves the expense of a production model. Curiosity is fine; just call it what it is and prototype it, don't operationalise it.

A huge share of "AI requests" I've seen die right here. Someone wants a prediction, but the moment you trace it forward, the real need is a number on a screen that a human already knows how to interpret. That's a SQL query and a chart, not a model. Ship those, and you've solved it this week instead of this quarter.

## Test 2 — Is the pattern already obvious to the people in the room?

A model earns its keep when the signal is **invisible to a human and expensive to get wrong** — that's the whole justification. When there are hundreds of interacting features, or the relationship genuinely shifts over time, modelling finds structure nobody could see by eye.

When there are only a handful of conditions, and a domain expert could write the rule on a whiteboard in ten minutes, a model is a liability. You've taken something transparent and made it opaque, slower, and harder to defend. The business user now has a *reason code* they can't explain, an auditor has a black box they can't sign off, and the moment the model does something odd, nobody can tell you why. A simple rule you can read, argue with, and change on a Tuesday afternoon is worth more than an accurate model you have to retrain and re-litigate. "Would a spreadsheet answer this?" is a surprisingly sharp question. If yes, it usually should.

## Test 3 — Would a human do it about as well, and the volume is low?

Even when a model *could* work, automation only pays off at volume. If a forecast is needed for thirty accounts and an analyst with a decent spreadsheet is 90% as good, the last 10% of accuracy is not worth the entire machine — the labelling, the pipeline, the monitoring, the drift. Automate what's high-volume and repetitive. Leave the low-volume, high-judgment calls to humans and give them better tools and better data instead of a model.

## The part nobody puts in the pitch deck

Models aren't a build cost; they're a **maintenance cost that never stops**. A model in production is an assumption about the world that quietly rots the day the world changes. Every one you ship is a recurring commitment: data pipelines that must keep feeding it, features that must stay available, thresholds that must be monitored, drift that must be detected and corrected, and a human who must own all of that when the original author has moved on. I've watched teams spend a quarter getting a model to work and then three years keeping it from falling over — for a problem a rules table would have covered on day one.

And there's the trust cost. The moment a model gets a visible case wrong, the people you're trying to help stop believing the whole thing, and you spend your credibility defending complexity instead of delivering clarity. A transparent rule that's occasionally wrong in an *explainable* way holds trust far better than a slightly-more-accurate model that's occasionally wrong in a *mysterious* way.

## What "earning the model" actually looks like

I'm not anti-model. I build them. But I want them to survive scrutiny, so I give them a fair trial:

- A **baseline** that's embarrassingly simple — often just the mean, or the rule the SME would write. If a gradient-boosted forest beats it by a rounding error, the model loses.
- A **measure tied to the decision from Test 1**, not an accuracy metric the business can't feel. Precision on a chart is not the same as money saved or risk avoided.
- An honest **total-cost estimate**: build plus the maintenance forever, weighed against the value of that last sliver of improvement.

Run that gauntlet and you'll find models are occasionally, genuinely the right call — and when they clear it, everyone trusts them precisely *because* they had to fight for their place.

The mindset I'm pushing for is simple: treat a model as an expensive answer, not a default one. Most of good analytics is getting clean, well-understood data in front of a thoughtful human with a transparent view — and then only reaching for the model where the invisible-and-expensive condition truly holds. Fewer models, better models.

Next time I'll take the other side of the ledger: the **anatomy of a model that actually earned its keep** — what made it worth every bit of the maintenance, start to finish.
