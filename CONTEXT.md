# unfold context

unfold is the orient/decide layer of an OODA-loop reading of the practice of pairing with coding agents. The site captures observations from real work, distils them against prior learning, and produces decisions that fold into other projects. The companion site `legible` (not yet built) will hold the act layer — patterns mature enough to be instantiated as worked artefacts across multiple projects.

## Language

**Observation surface**:
The set of work-streams unfold scans for material — own projects (tdf26, simpson-registers, registers-framework) and others' published practice (mattpocock-skills, Claude Code skills marketplaces, other practitioners' repos).
_Avoid_: input pipeline, intake, inflow source.

**Note candidate**:
A captured observation in GitHub-issue form, not yet drafted to a note. Queue is inflow-driven from project retros and from the cross-project scan.
_Avoid_: backlog item, ticket.

**Note**:
A published Markdown distillation of a single practice or observation, in `notes/`. Legible to a reader who does not know the source project.
_Avoid_: post, article, entry.

**Fold event**:
The closure of an OODA loop — an unfold artefact (note, candidate, memory entry, ceremony observation) shaped a decision in another project, a skill, a hook, a setup step, or a convention. The unit of progress.
_Avoid_: adoption, application, use.

**Failure-to-fold**:
A re-discovery that should have been prevented; a captured pattern ignored when it should have applied; a downstream project blocked because the unfold-side artefact did not exist. The strongest progress signal.
_Avoid_: misfire, miss, slip.

**Silent fold**:
A fold that absorbed into practice without explicit mention in any retro, PR, or commit. Best outcome by design — and invisible to direct counting.
_Avoid_: stealth adoption, organic uptake.

**Capture quality**:
A property of a captured observation — whether it sits at the right grain, with clear unlock criteria, in a form that can fold cleanly when the moment arrives.
_Avoid_: note quality, ticket quality.

**Promotion** (unfold → legible):
The mechanism (not yet built) for moving a note from unfold to the future `legible` site after sustained use across multiple projects. Distinct from *fold event* — folds happen at any stage; promotion is specifically the unfold → legible transition.
_Avoid_: graduation.

**Cross-project scan**:
Pre-retro methodology that surveys multiple project surfaces (own + others') and classifies observations against the existing queue. Methodology at `~/.claude/plans/sharded-finding-sloth.md`.
_Avoid_: review, sweep.

**Threads** (retrospective lens):
Three loose roadmap themes — A (Voices architecture), B (Note-candidate triage & promotion), C (Framework provenance) — used as a retrospective lens for reading a cycle's shape, *not* as an active routing mechanism for sprint items.
_Avoid_: tracks, swimlanes, epics.

**Ownership voices** (Piers, Tully):
Named identities with tie-breaking authority for their owned repo — Piers for unfold, Tully for tdf26. Distinct from risk voices and from authorial-register voices.
_Avoid_: agent, persona (in this context).

**Risk voices** (Value, Usability, Feasibility, Viability):
The four SVPG product-risk lenses, instantiated as Claude Code subagents at `.claude/agents/`. Used for critique of a note draft, design, or decision. Distinct from ownership voices.
_Avoid_: critics, reviewers.

## Relationships

- An **observation** surfaces from an **observation surface** and may be captured as a **note candidate**.
- A **note candidate** may be drafted to a **note**, may close as a **fold event** from candidate state, may be closed as a **failure-to-fold** that triggers other work, or may sit indefinitely waiting for its unlock criteria.
- A **fold event** is independent of a **note** — an observation can fold into practice from candidate state without ever earning a note.
- A **note** can earn its way to `legible` only via **promotion**, which presupposes accumulated **fold events** across multiple projects.
- **Capture quality** is a precondition for clean folding; judged at capture time, re-evaluated at retro.
- **Failure-to-fold** is the primary signal a retro reads for; **silent folds** are accepted as unmeasurable.

## Example dialogue

> **Justin:** Did #35 fold this cycle?
>
> **Piers:** No fold yet. v1.4.11 reinforced the central claim but added no new policy variants — that's a defer, not a failure-to-fold. Capture quality is fine; the pattern is waiting for a new policy variant before drafting is justified.
>
> **Justin:** And #44 — the mechanics-explainer?
>
> **Piers:** Failure-to-fold in the live system. The tdf26 wiki page is currently blocked on unfold publishing this. Strongest progress signal in the open queue.

## Flagged ambiguities

- **"voice"** is used in three distinct senses: *authorial voice / register* (from `registers-framework`), *ownership voice* (Piers/Tully), and *risk voice* (SVPG four-risks). Memory entry `feedback_voice_terminology.md` pins the distinctions. Do not collapse.

- **"persona" vs "voice" vs "agent"** were used interchangeably in early notes; cycle-1 retro Q5 settled canonical usage. See `feedback_voice_terminology.md`.

- **"promotion"** was overloaded in three senses: (1) candidate → note, (2) note → framework page, (3) note → legible. CLAUDE.md's *"Promotion principle: fork, don't rewrite"* applies to both (2) and (3): a later-stage page is a fresh distillation, not a rewrite of the earlier-stage artefact. When used as a bare noun, *promotion* in this glossary means (3) — unfold → legible — because (1) is more precisely *drafting from a candidate* and (2) is more precisely *distilling a framework page*. The fork-don't-rewrite discipline is the same in both senses.
