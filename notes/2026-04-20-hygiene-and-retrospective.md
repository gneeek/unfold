---
layout: page
title: "Hygiene and retrospective: different mechanisms"
date: 2026-04-20
status: published
format: summary
related:
  - 2026-04-20-git-status-as-load-bearing-signal.md
  - 2026-04-20-session-end-at-seams.md
tags:
  - hygiene
  - retrospective
  - ceremonies
---

# Hygiene and retrospective: different mechanisms

Hygiene checks and retrospectives are both ways to notice things. They are not interchangeable, and when something goes wrong the instinct to reach for "more retro" is sometimes the wrong lever.

The distinction is positional. A **hygiene check** runs *before* the action that depends on it; it verifies that current state is what the upcoming action assumes. A **retrospective** runs *after* the action; it captures what happened, what it meant, and what to keep or change. Both are reflective practices. Only one of them intercepts.

## The question to ask

When something has gone wrong, the useful framing is:

**Is the fix learning-shaped or verifying-shaped?**

If the fix is "understand what we didn't know" — retrospective. Something was invisible to the people involved until it bit them; the work is to see it more clearly next time.

If the fix is "catch this before it happens again" — hygiene. The failure mode is knowable, and the work is to verify state before the action that would decay under the missed detail.

Reaching for "more retro" when the problem is verifying-shaped is an easy mistake because reflection *feels* productive. A retrospective run after the incident, led by the same operator who made the mistake, will re-encounter the same blindspot. The seat is the same seat. The operator who missed the signal at the time will not see it in the reflection either.

## Worked example

On tdf26, a small project that publishes reflective entries on a cycling tour, a commit made in one evening session was wiped later the same evening by a destructive git operation from an unrelated workstream. The commit was only local — it had not been pushed — so the wipe was final.

The initial retrospective instinct was "every release should get a retro so this kind of thing gets caught." That framing is learning-shaped: more reflection, better pattern-recognition, fewer incidents.

But a retrospective the following day would not have caught this particular failure. The operator who ran the destructive command was the same operator who would have run the retrospective. Same seat, same blindspot. The reflection would have surfaced something — probably "I should have been more careful" — without actually raising the chance of catching the next one.

The real fix was verifying-shaped. Two hygiene patterns, not one:

- Before any destructive git operation, verify `git status` is clean and that no local-only commits sit unpushed. The check is mechanical. It does not depend on the operator's state of mind.
- End the session at the seam between unrelated workstreams so the next one starts with a clean state-read. See [deliberate session-end at workstream seams](./2026-04-20-session-end-at-seams.md) for that pattern, and [keeping `git status` load-bearing](./2026-04-20-git-status-as-load-bearing-signal.md) for the signal discipline both patterns depend on.

Both practices are valuable. Only one of them intercepts the failure being fixed.

## Warning

Confusing the two is the default. Retrospectives *feel* productive — reflection, pattern-recognition, improvement as a visible activity. Hygiene checks feel mechanical — a `git status` glance, a checklist item, friction. The felt productivity of the reflective mode pulls attention toward it even when the fix needs to be mechanical.

A useful frame: **reflection is what you do to catch surprises; verification is what you do to survive known failure modes.** A project with plenty of retrospective and no hygiene is a project that knows itself well and loses work anyway.

## Related

- [Keeping `git status` load-bearing](./2026-04-20-git-status-as-load-bearing-signal.md) — the hygiene pattern at the state-signal level
- [Deliberate session-end at workstream seams](./2026-04-20-session-end-at-seams.md) — the hygiene pattern at the session-boundary level
