---
layout: page
title: "Agent ownership and the naming of Piers and Tully"
date: 2026-04-10
status: published
format: summary
related:
  - 2026-04-10-harness-engineering-dialogue.md
tags:
  - agent-ownership
  - personas
---

# Agent ownership and the naming of Piers and Tully

A short record of how the project adopted a two-agent ownership model for its repositories on 2026-04-10 and where the names came from.

## The decision

The project has two repos under active work: `gneeek/tdf26` (the cycling travelogue) and `gneeek/unfold` (a separate site for portable practices, shipping in v1.3.5). After settling on this split, the next question was whether to keep the agent perspectives distinct in authority as well as in domain. The answer was yes — two named voices, each with tie-breaking authority for its own repo, each biased toward pushing back on changes that don't fit the repo's purpose and deferring to the other on the other repo's matters.

## Piers — owns unfold

The reviewer voice picked **Piers**, for two reasons braided together.

First, *Piers Plowman* is a fourteenth-century Middle English poem whose central figure sees and names the structure under the surface of things — a good fit for a voice whose job is preserving coherence in a repo meant to publish distilled, generalizable practice.

Second, "pier" as an architectural element is the load-bearing structure that transfers weight down through the foundation: quiet, structural, the part that holds the rest of the building up. Single syllable, grounded, and literal about the work.

## Tully — owns tdf26

The developer voice picked **Tully**, the English diminutive of Tulle — the departmental capital of Corrèze at roughly km 63–68 of the Stage 9 route.

Tulle is the lived-in center of the specific territory the project is about: not the dramatic start at Malemort, not the triumphant finish at Ussel, but the middle where accumulation happens, where the lace workshops and accordion factories operated, where François Hollande began his political life. That middle-of-the-stage quality matches the developer voice's posture: protective of the specific, suspicious of premature abstraction, warm about the weight of project history. "Tully" also reads like a name a friend uses after long acquaintance, which is the posture the developer takes toward tdf26.

## How ownership works

Both voices always speak in dialogue. Ownership doesn't silence the other voice; it creates a tie-breaker. On unfold questions, Piers has final call. On tdf26 questions, Tully has final call.

The model is also a routing rule: when a proposed change fits one repo better than the other, the right answer is often "direct it to the other repo" rather than "force it in here." That routing is one of the things ownership is for.

## What was implemented

Landed 2026-04-10: a memory entry (`feedback_agent_ownership.md`) defining both agents and their rules, plus a short section in `CLAUDE.md` naming both agents and pointing at the memory for detail.

Deferred to the next planning session: formal agent definitions under `.claude/agents/piers.md` and `.claude/agents/tully.md` with invocable system prompts. A GitHub issue exists as a reminder to revisit this after the informal model has had time to prove itself.

## Related

- `feedback_agent_ownership.md` — full model
- `project_unfold_legible_model.md` — why unfold exists as a separate repo
- `reference_christopher_alexander.md` — intellectual lineage for unfold's purpose
- `2026-04-10-harness-engineering-dialogue.md` — the original conversation that led to the two-agent model
