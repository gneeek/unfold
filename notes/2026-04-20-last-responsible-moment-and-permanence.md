---
layout: page
title: "The last responsible moment under a permanence rule"
date: 2026-04-20
status: published
format: summary
sources:
  - https://www.oreilly.com/library/view/lean-software-development/0321150783/
related:
  - 2026-04-20-ceremonies-on-the-ooda-loop.md
---

# The last responsible moment under a permanence rule

Mary and Tom Poppendieck's *Lean Software Development* names the **last responsible moment** (LRM): delay a decision until the cost of delaying further outweighs the cost of deciding now. Held back, decisions benefit from information that arrives during the wait. Held too long, the decision is no longer available at all.

Applied honestly, LRM is a judgement call. The operator weighs "what might I learn if I wait another day?" against "what have I given up by not committing?" The honest version is hard to run under time pressure. The easy failure mode is to slide past the LRM — neither deciding nor holding, just letting the moment pass while appearing still to be deliberating.

The refinement proposed here: **a permanence rule defines the last responsible moment sharply. Without such a rule, the LRM is a judgement call; with one, the moment is unambiguous and the scrutiny window has a natural place to live.**

## What counts as a permanence rule

A permanence rule is any project convention that makes something irreversible at a specific point:

- A content-change rule: "once an entry is published, its content is not retroactively edited."
- An append-only log: once written, entries are not deleted or amended.
- An immutable tag: once a release tag is pushed, it does not move.
- A signed commit or audit record: once recorded, tampering is detectable.
- A published API contract: once a version is released, its shape does not change.

What these have in common is a **point of no return**. Before the point, decisions remain cheap. After, they are either impossible or visibly expensive. The rule creates a cliff; the LRM is the edge of the cliff.

## Why this sharpens the judgement call

Without a permanence rule, sliding past the LRM is survivable. The decision can still be made later, at higher cost but not at infinite cost. Because it is survivable, the review window has no structural weight, and vigilance is the only thing stopping operators from drifting past it. Vigilance has well-documented failure modes — attention decay, fatigue, familiarity blindness.

With a permanence rule, sliding past the LRM is not survivable. The review window before the point of no return is load-bearing not because the operator remembers to make it so, but because the rule enforces it. The same review, with the same operator, has different structural weight under the two regimes.

## Motivating incident

On tdf26, a small project that publishes reflective entries on a cycling tour, the content-change rule is "once a segment entry is published, its content is not retroactively edited — corrections are carried forward into subsequent entries, visibly, rather than silently patched into the past." The rule was adopted to protect a reader commitment about what publishing means.

For one release, a pre-publish review window opened between PR merge and the run of the deploy script. In that window, factual cross-checks caught three errors — a wrong direction of travel, a wrong administrative boundary, and a mis-assigned segment number. Any of the three would have locked into the published entry permanently under the content-change rule, fixable only by issuing a visible correction in a later entry.

The review was load-bearing, not ceremonial. But what made it reliably load-bearing was not operator vigilance. It was the content-change rule, which defined the deploy script as the moment of permanence, and the window before it as where late-improving decisions belong. Without the rule, the same review could have been skipped with no permanent consequence — fix later, retroactively, no reader-visible trace. With the rule, skipping the review would have committed the errors to the record. The review window had structural weight because the permanence rule gave it some.

## Generalization

The pattern applies wherever a project has a real permanence boundary. The question to ask about any in-progress system is:

**What is the permanence rule here, and is the window before it being worked?**

- An append-only log has a permanence rule at each write. Code review of entries before they land is the LRM window.
- An immutable release tag has a permanence rule at each tag push. The pre-tag review (changelog checks, version bumps, release-note drafting) is the LRM window.
- A published API contract has a permanence rule at each version release. The contract review before a version ships is the LRM window.
- A signed audit record has a permanence rule at each signature. The review before signing is the LRM window.

The permanence rule does not have to be heavy or formal. "Once I send this message, I cannot unsend it" is a real-enough permanence rule that messaging clients have evolved undo-send windows and read-receipts-off conventions around exactly this LRM.

## What this is not

This is not an argument for inventing permanence rules where none naturally exist. Adding artificial irreversibility for its own sake is how projects acquire ceremonial pre-flight checks that serve no structural purpose. The pattern works only where the permanence is real — where the cost of reversal is high enough that the review window has weight to carry.

It is also not an argument that vigilance is obsolete. Permanence rules do not do the reviewing themselves; they create a window where reviewing pays off. The work of the review is still the work of the review.

## Credits

The *last responsible moment* vocabulary is from Mary and Tom Poppendieck, *Lean Software Development* (2003). The refinement here — that a permanence rule defines the LRM sharply and gives the review window its structural weight — is the contribution.

## Related

- [Ceremonies on the OODA loop](./2026-04-20-ceremonies-on-the-ooda-loop.md) — a cycle that ends at an LRM; this note names the invariant that makes the end of the cycle load-bearing
