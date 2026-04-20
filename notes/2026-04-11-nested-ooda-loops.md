---
layout: page
title: "Loops inside loops: OODA at multiple scales"
date: 2026-04-11
status: published
format: summary
related:
  - 2026-04-10-agent-ownership-and-names.md
---

# Loops inside loops: OODA at multiple scales

A short note on an observation made during v1.3.5 planning on the tdf26 project and refined during the v1.3.5 retrospective. The observation is that our reflective practice — retrospective, planning, sprint — maps onto John Boyd's OODA framing not at a single scale but at several nested scales at once, and that this is not our invention but something already implicit in the original framing.

## OODA, briefly

Boyd's [OODA loop](https://en.wikipedia.org/wiki/OODA_loop) names four steps — Observe, Orient, Decide, Act — and claims that running through them faster than the environment changes is the decisive advantage in any fast-moving situation. The framing comes from fighter-combat tactics but is used wherever speed of adaptation matters more than depth of plan.

## The macro mapping

On tdf26, work cycles through three phases between releases. A **retrospective** after a release ships, a **planning session** for the next release, a **sprint** that executes that plan. These map roughly onto OODA at the macro scale. The retrospective is Observe: what actually happened, what felt good, what felt bad. The planning session is Orient-plus-Decide: given the observation, what does the landscape look like now, and which next commitment is worth making. The sprint is Act: shipping the chosen scope.

The mapping is loose. A retrospective is not *only* observation, and a planning session is not *only* decision. The fit is directional, not exact.

## The refinement: each step is itself a loop

What made the mapping click was noticing that each macro step contains its own small OODA loop. A retrospective, at the macro scale, is Observation. But internally, a retrospective runs its own observe-orient-decide-act: look at the diffs and the ceremony logs (observe), frame them in four sections (orient), choose what to write down versus discard (decide), commit the page to the wiki (act). A planning session does the same: read the open issues and the planning-notes memory (observe), place them against the roadmap and the calendar (orient), pick one scope (decide), file the tracking issues and set up the milestone (act).

The whole cycle is meant to be a fast OODA loop. Inside each step, a smaller local OODA loop. Inside that, smaller still, if the step is large enough to warrant it. The structure is fractal.

## Reconciling three framings

Before the nested observation, tdf26 had three framings for its work: *retro → plan → sprint*, a set of [three work loops](/unfold/framework/work-loops/) (problem selection, solution exploration, delivery choices), and a general claim that we try to run *fast OODA loops*. These three framings overlapped enough to be confusing and diverged enough to be actually different things.

The nested reading reconciles them. The three work loops are three internal OODA loops running at different scales. Retro-plan-sprint is one specific timescale of that loop. The fast-OODA claim is what the whole structure is trying to be. Three views of the same recursive pattern.

## This is recovery, not invention

Boyd's original framing already described OODA at multiple timescales. A pilot runs OODA several times per second; a commander runs it at daily or weekly scope; a doctrine runs it over years. The nested reading here is not new philosophy; it is noticing that Boyd's own vocabulary already handled the scale question, and recovering that part of the source material for a small reflective project that had been using the framing informally.

## Takeaway

When a retrospective feels like it is about to skip a step, the useful question is which of the four substeps is being skipped. When a planning session dives into solutions before orienting on the landscape, the same question. The discipline transfers at every scale at which it is applied, and that is what "fast OODA" has always meant.

## Related

- [Agent ownership and the naming of Piers and Tully](./2026-04-10-agent-ownership-and-names.md) — the two-agent model that runs these loops in dialogue
