---
layout: page
title: "Deliberate session-end at workstream seams"
date: 2026-04-20
status: published
format: summary
related:
  - 2026-04-20-hygiene-and-retrospective.md
  - 2026-04-11-parallel-branches-parallel-sessions.md
---

# Deliberate session-end at workstream seams

When a single agent session — Claude Code, or any similar long-running context — spans multiple unrelated workstreams, the seams between workstreams are where mistakes happen. State from one workstream bleeds into the next. Destructive commands from the second workstream find uncommitted work from the first. The operator's sense of "what is on disk right now" is half a context behind.

A **session seam** is the moment between one workstream ending and another beginning. Deliberately ending the session at that moment converts an accidental boundary into an intentional checkpoint.

## The mechanism

The hygiene benefit of ending a session is that the next session *has* to start from scratch. A new session requires `git status`, memory-read, state-verification — the exact hygiene that mid-session workstream-switching bypasses. The session-end discipline creates the hygiene check on resume, rather than relying on the operator to remember to run it.

This is a structural counterpart to the operator-behaviour rule "commit or stash before switching workstreams." The behavioural rule gets forgotten. The session-end discipline does not, because starting a new session forces re-orientation whether or not the operator was planning to re-orient.

## When to end, when to continue

The judgement is: *would losing this conversation's context cost more than the hygiene benefit of a clean restart?*

- **End** after a discrete deliverable lands — committed, pushed, PR merged or ready for review, deployed if applicable. That is the completion punctuation.
- **End** before starting an unrelated workstream — a different goal, a different branch, a different kind of work (prose vs. a dependabot PR vs. a bug fix).
- **Don't end** mid-deliverable. The conversation's built-up context has real value when you are still extending the same piece of work. Ending mid-flight costs re-orientation without a boundary to anchor on.

For multi-workstream evenings, the answer is almost always *end*. For sub-tasks that are genuinely continuous with what came before, the answer is *continue*.

## Worked example

On tdf26, a small project that publishes reflective entries on a cycling tour, one evening session handled three workstreams in sequence: a content hotfix for a broken entry, a dependabot PR resolution on unrelated code, and drafting the next week's narrative entry. The narrative draft was committed locally between the hotfix and the dependabot work.

During the dependabot cleanup, a destructive git operation ran and took the narrative commit with it. The commit had been made in a context the operator was no longer in by the time the destructive command ran — the session had rolled on to something else, the mental model had moved, and the commit was invisible to the operator's current frame.

Deliberately ending the session after the hotfix would have changed the outcome. The dependabot work would have begun in a fresh session, which would have started with `git status` — surfacing the unpushed commit as a known unfinished thread before any destructive operation ran. The failure required the three workstreams to share one continuous frame.

## Warning

The pattern is especially easy to miss because ending a session feels wasteful. The conversation has scrollback. The context has been built. Re-establishing it costs time. The felt cost is immediate; the saved incident is counterfactual.

A useful frame: *session cost is not the same as context cost.* Starting a session is cheap; a built context is only expensive to lose if the work remaining genuinely depends on it. For an unrelated workstream, the work almost never does.

## Related

- [Parallel branches, parallel sessions](./2026-04-11-parallel-branches-parallel-sessions.md) — separating workstreams in space (branches, worktrees); this note is the same discipline applied in time
- [Hygiene and retrospective: different mechanisms](./2026-04-20-hygiene-and-retrospective.md) — why session-end discipline is a hygiene pattern rather than a reflective one
