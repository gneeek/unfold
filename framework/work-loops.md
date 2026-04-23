---
layout: page
title: Three work loops
permalink: /framework/work-loops/
---

# Three work loops

Three modes of work that should not be conflated:

1. **Problem selection** — choosing what is worth working on.
2. **Solution exploration** — choosing how to solve a problem once selected.
3. **Delivery** — choosing how to ship the chosen solution.

Each is a recurring loop. Feedback from delivery flows back into problem selection; feedback from solution exploration flows back into problem selection too, when the exploration reveals that the problem was wrongly posed. The three loops run at their own natural tempo, and a healthy practice keeps them distinct — mixing them is where scope creep, premature commitment, and stalled work all originate.

## Why not conflate them

Each loop answers a different question, and answers them in a different frame of mind.

- Asking *"is this worth working on?"* is an **open** question — the work is to consider alternatives, weigh cost against value, and reject more than is accepted.
- Asking *"how should I solve it?"* is **exploratory** — the problem is settled, but the solution space is still wide.
- Asking *"how do I ship this?"* is **executional** — the shape is settled, and the work is to land the change cleanly.

A planning session that wanders into solution detail has left the first loop and entered the second, usually without noticing. A sprint that reopens the problem has jumped from the third loop back to the first, usually because the second loop was skipped. Each such jump is cheap in isolation and expensive in aggregate: it is the shape of most of the waste in small-team software work.

## Each loop is an OODA loop

The three-loop framing sits alongside two other framings used on the originating project: *retro → plan → sprint* as a ceremony cadence, and *fast OODA loops* as a general claim about how the work is meant to move. These three framings overlapped enough to be confusing and diverged enough to be actually different things.

The reconciliation is that each work loop is itself an OODA loop — Observe, Orient, Decide, Act — running at its own scale:

- **Problem selection** runs OODA over weeks-to-months: observe what the project and its users are doing, orient on what matters, decide what to take on next, act by committing it to a roadmap or a release plan.
- **Solution exploration** runs OODA over days-to-weeks: observe the problem shape, orient on the options, decide on an approach, act by drafting the change.
- **Delivery** runs OODA over minutes-to-hours: observe current state, orient on what remains, decide the next step, act by making it.

The retro-plan-sprint ceremony cadence is one specific timescale of this pattern. *Fast OODA* is what the whole nested structure is trying to be. Three views of the same recursive shape.

## Practical consequences

1. **Name the loop you are in.** When a conversation stalls, it is often because two people are running different loops — one is still selecting problems, the other is already exploring solutions. Saying which loop surfaces the mismatch.
2. **Do not short-circuit the loops.** Skipping problem selection and going straight to exploration commits effort to work that may not be worth doing. Skipping exploration and going straight to delivery ships the first idea that appeared, not the best one.
3. **Let delivery feed problem selection.** What ships teaches you what to work on next. A delivery loop that does not feed back into problem selection is running open-loop, and the project drifts away from what its actual use is telling it.

## Origin

The three-loops framing is inspired by Marty Cagan's **product operating model**, as written up by [Silicon Valley Product Group](https://www.svpg.com/). SVPG's treatment of product discovery and delivery as distinct modes, and its emphasis on problem selection as its own concern, shaped the decomposition used here. The OODA overlay and the ceremony-cadence mapping are this project's additions.

This framework was implicit in the project for some time before it was named. Two notes in the working material preceded and shaped this page:

- [Loops inside loops: OODA at multiple scales](/unfold/notes/2026-04-11-nested-ooda-loops/) — the observation that the project's reflective practice runs OODA at several nested scales at once, and that the three-loops framing and retro-plan-sprint are two views of that nested structure.
- [Ceremonies on the OODA loop: retro, planning, sprint](/unfold/notes/2026-04-20-ceremonies-on-the-ooda-loop/) — the sharper mapping of the ceremony cadence onto OODA, and why retro-before-planning is structural rather than conventional.

Those notes remain the archaeological record of how the framework was first observed; this page is a fresh distillation, not a rewrite of them.
