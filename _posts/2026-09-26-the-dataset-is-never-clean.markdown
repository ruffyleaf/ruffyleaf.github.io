---
layout: post
title:  "The dataset is never clean — a realistic first week on a messy dataset"
date:   2026-09-26 22:10:00 +0800
categories: data-analytics practice
tags: [data-quality, analytics, data-management, field-notes]
---

If you work with data long enough, you hear the same request in a dozen different clothes: *"just clean the data and give me a dashboard."* It sounds like a two-line task. It never is. The honest reality is that the dataset on your desk is already dirty in ways you can't yet see, and the people asking for it don't know which of those ways actually matter for the decision they're trying to make. So the first week isn't about cleaning anything. It's about finding out *what kind of dirty* you're dealing with, and which of it is safe to ignore.

Here's the order I actually work in. Not the textbook order — the one that survives contact with real data.

## Day 1 — Don't touch it. Just look.

The instinct is to open the file and start fixing. Resist it for a day. On a large, unfamiliar dataset, the single most damaging thing you can do is change a value before you understand what the value *means*. So day one is pure profiling. I'm answering three questions:

- **Shape.** How many rows, how many columns, and does the row count match what the business expects? A table that's supposed to hold every customer and somehow has fewer rows than there are active accounts is telling you something already.
- **Completeness.** What share of each column is populated? Nulls aren't uniformly bad — a column that's 4% empty is a rounding error, a column that's 60% empty is a story, and the story is almost never about the data.
- **Distributions.** For every column that matters, look at its range and its shape. Min, max, mean, median, the obvious outliers. If a "age" column tops out at 147, or a monetary column is full of exact repeats down to the cent, or a date column has records from 1970, you've just found the seams where the dataset was stitched together.

The output of day one isn't a fix. It's a list of surprises and a rough map of how bad it is.

## Day 2 — Profile like a detective, not a statistician

The difference between an experienced data person and a script is that the script reports *what* is wrong; you need to work out *why*. Sentinel values masquerading as data are the classic example — a `-1` or `9999` or a blank string that isn't really null but definitely isn't a real value. A `GROUP BY` on the "weird" columns and a quick eyeball of the top and bottom values surfaces these fast.

Duplicate keys are the second category worth real time. Is a duplicate a genuine data-quality bug (the same transaction captured twice) or a modelling question (the same customer legitimately placing two orders)? You cannot know that from the data alone, which is the segue into day three.

## Day 3 — Follow the lineage before you follow your gut

Most "cleaning" mistakes are really lineage mistakes. Before I dedupe, impute, or standardise anything, I want to know where each field *comes from*. Which upstream system owns it. Whether it's a daily snapshot or an append-only log. Who else downstream already depends on it.

This matters because the same field often means different things in different systems, and a value that looks broken in isolation is perfectly correct given how it was produced. A null that's genuinely "missing" and a null that means "the user skipped an optional field" look identical in the file and require opposite treatments. You only tell them apart by walking the data back to its source and asking the people who own it a small number of specific questions.

The other reason to do lineage early: once you know what downstream systems read this data, you also know what you're *not* allowed to change casually. That constraint shapes every decision after it.

## Day 4 — Fix the unambiguous things, in the open

Now you're allowed to fix something — but only the changes that are clearly right and cheap to reverse. Standardising formats (dates to one format, text to one case, units to one system). Collapsing obvious sentinel values into true nulls. Documenting every one of them in a single, boring, readable log that says *what you changed, why, and how to undo it.*

That log is the difference between a data professional and a person who quietly breaks a report. Nobody ever resents a well-documented change. They resent finding out three weeks later that the numbers moved and no one can say why.

## Day 5 — Handle the judgment calls as decisions, not accidents

The genuinely hard problems don't have a "correct" answer; they have a defensible one you chose on purpose. Do you impute the missing values, drop the rows, or leave the gap visible and let the consumer decide? Do you merge two records that are *probably* the same person, or keep them separate and accept the overcount? Each of these should be written down as a small, explicit decision with a rationale and a home (usually the same log), so that six months later you can explain the shape of the data to someone who wasn't in the room.

A good rule I use: make the choice that a reasonable colleague, reading only the dashboard and not the code, would find least misleading. Data engineering optimises for correctness; decision support optimises for trust. On a messy dataset you're doing both jobs at once.

## The checklist I keep

Stripped down, the first-week sequence is:

1. Row counts and key uniqueness against what the business expects.
2. Null / empty rate for every column.
3. Range and distribution for every column that feeds a decision.
4. Hunt for sentinel values and impossible dates.
5. Write a "what I'd expect vs. what I see" list.
6. Map each field to its source system and its downstream users.
7. Fix only the reversible, unambiguous problems — and log every one.
8. Turn the remaining mess into explicit, documented decisions.

The point of the whole week isn't to arrive at a clean dataset. It's to arrive at a dataset you *understand* — one where you know exactly which parts are trustworthy, which parts are a judgment call, and which parts you'd never bet a decision on. That's the honest version of "clean," and it's the only kind that actually holds up when someone asks you, a month later, *"can we rely on this?"*

Next time, I'll go the other way on purpose: **when NOT to build a model** — the framework I use for spotting when a simple dashboard or a plain rule earns its keep better than machine learning ever could.
