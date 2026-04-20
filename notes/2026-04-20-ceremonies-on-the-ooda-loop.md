---
layout: page
title: "Ceremonies on the OODA loop: retro, planning, sprint"
date: 2026-04-20
status: published
format: summary
related:
  - 2026-04-11-nested-ooda-loops.md
---

# Ceremonies on the OODA loop: retro, planning, sprint

A refinement of the earlier note on [nested OODA loops](./2026-04-11-nested-ooda-loops.md). That note named the inner loops running during each ceremony. This one names the outer loop that the ceremonies themselves implement — and finds that the mapping is sharper than the earlier note allowed.

## The sharper mapping

The earlier note called the mapping "loose" and "directional." It is not. Laid out against Boyd's four steps, the three ceremonies cover the full cycle with one deliberate overlap:

- **Retrospective = Observe + Orient, looking backwards.** What happened, what did it mean?
- **Planning = Orient + Decide, looking forwards.** Given where we now stand, what does the landscape imply for what is next, and what do we commit to?
- **Sprint = Act,** carrying its own inner OODA cycles during execution.

Orient appears in both retro and planning because orientation is the interpretive step. The past needs to be oriented on before it can be used; the future needs to be oriented on before a decision about it can be made. The two orientations are different work — one re-reading, one forward-projecting — but they share the interpretive character.

## Why retro-before-planning is structural

The overlap is the useful part. Planning without fresh orientation on the past pushes decisions off un-reconciled state. The orient step in the planning session has nothing to work with if the retro has not happened first, and the planner ends up either skipping orientation (deciding on stale ground) or improvising it under time pressure (reconciling and deciding in the same breath, badly).

Retro-before-planning is therefore not a schedule convention, and not a matter of ceremony etiquette. It is structural: the two ceremonies share a step, and the one that precedes does the work the other needs. Swapping the order breaks the thing.

## When to run a retro

A retrospective is triggered by the approach of the next planning session. Not by a calendar. Not by a reflection-worthy event. The signal is that planning is about to happen; the retro is where the orient work for that planning is done.

This reframes a question that had been sitting open for several cycles: what signals that a retro is due? The question assumes the retro is a standalone event that needs its own trigger. It is not. The retro is the first half of a two-ceremony pair, and the pair is triggered by the need to plan. Publisher instinct — "it feels like time to think about what is next" — remains the upstream cue, but once that instinct fires, the sequencing is deterministic: retro, then planning.

The corollary: a retro held long after its adjacent planning session is doing something other than orient work for that planning. Usually it is drifting toward observation-only — interesting but weightless, because the orient-and-decide it fed is already in the past.

## Motivating incident

The question "what triggers a retro?" had been open across several release cycles on tdf26, a small project that publishes reflective entries on a cycling tour. Earlier attempts had rejected calendar-driven retros (every release gets one) as dilutive, and under-specified signal-driven retros (run one when something happened worth reflecting on) as unreliable.

The resolution surfaced accidentally. The publisher raised "time for a retro I think?" in the same message that opened planning for the next release. The sequencing itself suggested the answer: there is no separate trigger to define, because the retro is not a separate event. The retro is the orient-backwards step of a cycle whose decide step lives in the planning session. The trigger for the retro is the decision to plan.

## Relationship to the inner loops

The earlier [nested OODA note](./2026-04-11-nested-ooda-loops.md) pointed out that each ceremony runs its own internal OODA cycle: a retrospective observes-orients-decides-acts over the span of an hour, even though it is itself only the observe-orient step of the outer cycle. Both readings are correct and live at once. The outer loop runs at release cadence; the inner loops run at minutes-to-hours; Boyd's original framing accommodates both, because OODA was always a fractal structure rather than a single-scale ritual.

The useful discipline is to know which loop you are in. A sprint feels like "act" at the outer scale but contains constant observation and orientation at the inner scale. A retrospective feels like pure reflection at the outer scale but makes dozens of small decisions internally about what to record and what to leave alone. Conflating the scales is where the ceremonies lose their edge.

## Takeaway

Three practical consequences follow from taking the mapping seriously:

1. **Do not plan without first orienting on the past.** If the retro has not happened, the planning session will either skip orient or do it badly.
2. **Do not run a retro far from its planning session.** The orient work has a decide step to feed; if the decide step is already made, the orient work has no outlet.
3. **Do not conflate inner and outer loops.** A sprint is "act" at release scale; inside it, the four-step discipline still applies at every task.

## Related

- [Loops inside loops: OODA at multiple scales](./2026-04-11-nested-ooda-loops.md) — the inner loops inside each ceremony; this note names the outer loop the ceremonies themselves form
