---
layout: post
title:  "When a model earns its keep: anatomy of an ML project worth building"
date:   2026-09-26 23:15:00 +0800
categories: data-analytics ai
tags: [machine-learning, analytics, decision-framework, field-notes]
---

The last post was about when *not* to build a model — the three tests a request has to survive before modelling is even on the table. This one is the other side of the ledger: what a model looks like when it genuinely clears the bar, and worth every hour of maintenance it will quietly demand for years. Because they do exist. The point was never "fewer models." The point is *models that earn their place*, and those have a recognisable shape if you know what to look for.

Here's the anatomy, bottom to top — because the way a good model gets built is almost always the reverse of how it gets pitched.

## It starts from a decision with a price on it

A model that earns its keep is anchored to one specific, expensive decision — and someone can tell you roughly what that decision is worth. Not "understand our customers better," but "this forecast changes how much stock we hold, and being wrong costs us about X a quarter." That dollar figure is the whole business case. It's what you'll measure the model against, what justifies the maintenance cost, and what tells you, honestly, when the model *isn't* pulling its weight and should be retired.

If nobody can price the decision, it's usually a curiosity project. Nothing wrong with that — but fund it as a prototype, not as production.

## The signal is real, invisible, and stable-ish

Three properties of the underlying pattern matter, and a worth-building model has all three:

The signal is **real** — there is genuinely more to know than the baseline already gives you. The embarrassing simple baseline (last value, the mean, the whiteboard rule) is *beatable by a meaningful margin*, not by a rounding error. If your fancy model beats the naïve forecast by a hair, it doesn't clear the bar; the hair isn't worth the machine.

The signal is **invisible to a human**. It lives in the interaction between dozens of features, or in a shape nobody could eyeball on a dashboard. This is the entire reason you reach for a model rather than a report — you're finding structure that isn't otherwise findable. If a domain expert could have written the rule, use their rule.

The signal is **stable enough to survive shipping**. Every model encodes an assumption about how the world works. When the world churns fast — a brand-new product with no history, a market mid-regime-change — that assumption rots before the model reaches production. A model earns its keep where the pattern has held long enough to learn *and* long enough to trust it next quarter.

## The data is understood, not just available

This is where my first post connects. A model is only as durable as your understanding of the data feeding it. A model worth building sits on a dataset you've actually profiled — you know its gaps, its sentinel values, its lineage, and which parts you'd bet a decision on. Fragile models are almost always a symptom of fragile data understanding: you built intelligence on top of something you never really inspected, and the model inherits every hidden flaw. When the data is understood, you can tell the difference between the model being clever and the model memorising a bug.

## There's a human who acts differently because of it

A model earns its keep only if it changes what someone *does*. This is the most-skipped step and the biggest cause of abandoned ML. Before building, I want to see the shape of that change: where the output appears, in front of the person who acts, at the moment they're about to act, in a form they trust enough to let it override their gut. A brilliant prediction buried in a report nobody opens is worth precisely nothing. The cheapest part of most successful model projects is the modelling; the expensive, decisive part is the last mile into a real workflow.

This is also why I care about **explainability as a requirement, not a nicety**. Not because stakeholders demand interpretability in the abstract, but because a person who acts on a model needs to sanity-check it. If the model says "deny this one" and can't give a reason the operator respects, they'll route around it — and now you've paid for a model you don't actually use.

## The maintenance is owned, from day one

Here's the honesty test that separates teams that ship good models from teams with model graveyards: before you build, name who owns it in a year, and roughly what keeping it healthy costs. A model is a running commitment — pipelines that must keep feeding it, features that must stay available, drift that must be monitored, retraining that must happen when the world moves. If nobody's budgeted for the boring forever-part, the model doesn't earn its keep; it accrues a debt someone will pay later at high interest.

I like to write the model's own "retirement criteria" down at the start: the conditions under which we'd knowingly switch it off. It forces the whole team to admit the model is a means to the priced decision from step one, not a thing that exists for its own sake.

## What this looks like in the wild

Put together, a worthy model usually reads like: *a specific, priced decision; a signal that's real, hidden, and reasonably stable; data we actually understand; a named human whose behaviour changes when the output appears; and an owned plan to keep it alive.* Miss any one of those and the model tends to be expensive theatre — accurate on the validation set, inert in the organisation.

When all of them line up, models are the best tool we have. They find what we can't see, at a scale and speed no human matches, and they quietly compound value every day they run. That's the version worth building. It's just not the version most requests turn out to be — which is exactly the point of making them fight for their place.

Next, I want to zoom back out to the layer underneath all of this — **making charts people actually trust**: the small design and labeling choices that decide whether a non-specialist leans on a dashboard or quietly ignores it.
