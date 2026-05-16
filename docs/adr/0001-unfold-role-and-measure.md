# 1. unfold's role within OODA, audience layering, and measure design

Date: 2026-05-16

## Status

Accepted (2026-05-16). First ceremony to read against this framing: retro-03 (2026-05-17).

## Context

unfold has been running for ~5 weeks (3 ceremony cycles). The project's measure of progress has been implicit: queue:promotion ratio (21:0 across cycles 2–3), supplemented by ceremony reliability. Cycle 3's sprint completed as "no candidate ready" against its only target.

A grill session on 2026-05-16 (the day before retro-03 fires) surfaced the load-bearing question: what is unfold's measure actually measuring? CLAUDE.md described the publishing model (unfold → legible, fork-don't-rewrite) but never stated what the project is for.

Four audience framings were considered: (1) Justin's future-self practice; (2) other practitioners working with coding agents; (3) the future `legible` site (unfold as feeder); (4) the cross-project bridge between Justin's projects.

Four value framings were considered: (a) reduced re-derivation cost; (b) portable vocabulary; (c) re-readable artefacts of the practice; (d) the operating model itself, with site as artefact.

Four measure framings were considered: decision-influence count; practice maturity by transferability; re-derivation avoidance; layered measure with asymmetric weighting.

## Decision

**Audience layering.** Justin's own future-self on subsequent projects (primary); other practitioners trying similar work (secondary); coding agents (tertiary, as map-readers). Transferability tests with other humans are explicitly *out of scope* for unfold — that belongs to `legible`.

**unfold's OODA layer.** unfold sits at *orient/decide*. legible will sit at *act*. The observation surface is broader than own work — it includes others' published practice (mattpocock-skills, Claude Code marketplaces, other practitioners' repos). The act of applying decisions belongs to other projects.

**Value.** Reduced re-derivation cost. unfold's job is to capture observations once, orient them against prior learning, and produce decisions that fold into practice — so the same insight does not have to be re-derived per project.

**Measure of progress.** Two-tier (capture quality + fold events), with asymmetric weighting:

- Failure-to-fold evidence is the strongest signal.
- Explicit successful adoption is a weak signal (could be facile).
- Silent folds are best but invisible by design; accepted as unmeasurable.

## Consequences

**Positive:**

- The queue:promotion ratio loses its load-bearing status. 21 open candidates is not necessarily a backlog; some may already be silent folds, some may be failure-to-fold evidence, some may be in-flight at correct pace.
- Retro questions attend to failure signals (strongest evidence) rather than performative success.
- The relationship with `legible` is operationally clear: `legible` holds *acts* (worked artefacts); unfold holds *orient/decide* (captured patterns ready to fold).
- The observation surface is explicitly broader than own projects.

**Negative / open:**

- The current ceremony cadence was designed against the implicit queue:promotion measure. It may need adjustment. Planning-04 is the first ceremony to test this.
- Promotion (unfold → legible) is now distinct from fold events. This was implicit before; making it explicit forces decisions about both that were comfortably deferred.
- "We shipped X notes this cycle" no longer reads as straightforward progress.

## Alternatives considered

- *Audience = other practitioners (option 2)*: rejected because current operating evidence does not support reader-uptake as load-bearing; transferability belongs to legible's later validation.
- *Value = portable vocabulary alone (option b)*: rejected because vocabulary is a means (the bridge that allows folds) not the end.
- *Measure = decision-influence count alone*: rejected because it under-weights capture work and over-weights performative adoption.
- *Measure = transferability tests*: rejected because that's the legible measure, not the unfold measure.

## Related

- `CLAUDE.md` — new "Project goal and measure" section, same change
- `CONTEXT.md` — terminology resolutions from the same grill (fold event, failure-to-fold, silent fold, capture quality, observation surface)
- `~/library/research/unfold-notes/2026-05-16-goal-and-measure-grill.md` — durable grill record
- 2026-05-17 retro-03 — first ceremony to use this framing
