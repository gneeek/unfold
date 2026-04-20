# unfold — Portable Practices Companion Site

## Project Overview

unfold is a working collection of notes, dialogues, and patterns from a practice of building software with coding agents. The site is where rough material lives alongside more solid material, and where the rough becomes less rough through use on real projects. Published via GitHub Pages at https://gneeek.github.io/unfold as a Jekyll static site.

Self-description from `index.md`: *"This site is for practices that reveal their shape by being built. Each note is an attempt to describe something only after doing it, not before."*

## Two-stage publishing model

Content on the site sits in one of two stages:

- **unfold** (this repo) — in-progress practice, observations from one or two projects, notes that may not yet be portable. New material starts here.
- **legible** (reserved, not yet created) — battle-tested patterns, observed working across multiple projects, stated with confidence. Material earns its way from unfold to legible after use on more than one project.

Naming and framing draw on Christopher Alexander's *A Pattern Language* and *The Nature of Order*: content **unfolds** through piecemeal growth and structure-preserving transformations until it reaches a stable shape. Only then can it be made **legible**. The two-stage model gives in-progress thinking a venue without diluting the standard of mature practice.

**Promotion principle: fork, don't rewrite.** When a note earns its way to a later-stage page (patterns, legible), the original note stays in place as the archaeological record of how the observation first arrived. The later-stage page is a fresh distillation that references the note as origin, not a rewrite of it. This preserves the roughness that makes notes useful for future pattern-finding, and avoids the temptation to smooth away the incident-specific texture that makes the original observation legible. No mechanism is built for promotion yet — the first real promotion will drive the shape of that mechanism.

## Content structure

- `index.md` — home page
- `framework/` — framework pages (`five-goals.md`, `work-loops.md`, `pr-workflow.md`) and their landing index, drafts that are filled in as the patterns unfold
- `notes/` — individual note entries, each a distillation of a single practice or observation, grounded in observed incidents from consuming projects

Each note is written so a reader who does not know the source project can still apply the pattern. When a note references a motivating incident, the incident is summarised in enough detail to be legible but the source project is not assumed knowledge.

## Note conventions

Each note in `notes/` follows a consistent shape:

- **Filename:** `notes/YYYY-MM-DD-slug.md`. Date is the note's authored date; slug is kebab-case.
- **Frontmatter:** `layout: page`, quoted `title`, `date`, `status`, and `format` (one of `summary`, `explainer`, `dialogue` — extend only when a new shape genuinely does not fit). Optional: `related:` (a list of other note filenames), `sources:` (a list of URLs), `tags:` (a list of short topic keywords; no tag-navigation page yet, but the field is there so clustering can happen later without restructuring).
- **Index:** every new note adds a bullet to `notes/index.md`, newest last, with a one-line hook.

## Relationship to tdf26

unfold is a separate repository and a separate Claude Code project from `gneeek/tdf26`. They are related — tdf26 retrospectives and sessions regularly surface portable observations that are candidates for unfold notes — but they do not share a release cadence, a milestone system, or a roadmap. Observations from tdf26 arrive here as issues with a `Note candidate:` title prefix and a link back to the tdf26 retrospective that surfaced them; see the live issue list for current candidates.

See `feedback_unfold_tdf26_work_fit.md` in the memory system for the full separation rules.

## Agent Ownership

This project uses a two-agent ownership model shared with tdf26:

- **Piers** owns `gneeek/unfold` (this repo). Biased toward structure, distillation, and what travels across projects. Has tie-breaking authority on unfold decisions.
- **Tully** owns `gneeek/tdf26`. Biased toward project-specific craft, voice, and publisher experience. Has tie-breaking authority on tdf26 decisions.

Both voices contribute to any discussion. The owning voice's judgment is authoritative for its repo. When a proposed change fits one repo better than the other, the right answer is often "direct it to the other repo" rather than "force it in here."

See `feedback_agent_ownership.md` in the memory system for the full model including push-back criteria and deference rules.

## Tech Stack

- Jekyll static site (GitHub Pages)
- Content in Markdown with YAML frontmatter
- `_config.yml` configures Jekyll with the `minima` theme and pretty permalinks (`baseurl: /unfold`)
- No build step beyond Jekyll's own; deployed automatically by GitHub Pages on push to `main`

## License

All content is licensed under Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0), matching tdf26 content licensing. See `LICENSE`.

## Provenance of this file

Drafted from the tdf26 seat during v1.4.6 planning on 2026-04-20 as preparation for parallel unfold work, and refined from this repo's seat the same day: the enumerated issue-number list under "Relationship to tdf26" was dropped in favour of a pointer to the live issue list (numbers go stale fast), and a "Note conventions" section was added to capture the filename, frontmatter, and index-update patterns that were already implicit across the existing notes. Further refinement welcome as unfold's shape evolves.
