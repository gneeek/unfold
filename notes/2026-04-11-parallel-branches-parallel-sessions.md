---
layout: page
title: "Parallel branches, parallel sessions"
date: 2026-04-11
status: published
format: summary
related:
  - 2026-04-11-nested-ooda-loops.md
  - 2026-04-10-personas-vs-subagents.md
tags:
  - git
  - session-management
  - claude-code
---

# Parallel branches, parallel sessions

Two patterns that emerged on the same day during the v1.4.0 sprint on tdf26, both solving the same underlying problem: how to keep two streams of work moving without either one blocking or breaking the other. The shared principle is simple — make the work surfaces explicitly disjoint, and the coordination that remains becomes cheap rather than blocking.

## Pattern 1: worktrees over stash-and-switch

The situation: a dev server is running on a feature branch. A bug surfaces on a different issue. The instinct is to stash the current work, switch branches, fix the bug, switch back, pop the stash.

On tdf26, this broke things. The branch switch removed a content file from disk that the running dev server expected. The server crashed, the admin UI tried to write to the missing file, and recovery took a round-trip that cost calendar time on a tight deadline.

The fix was `git worktree add /path/to/worktree fix-branch`, which checks out the fix branch in a separate directory while leaving the original branch and its dev server untouched. The two branches coexist on disk. Neither knows the other is there. No stash, no switch, no crash.

**When to use it:** any time you need to work on a different branch while a dev server, test runner, or long build is running on the current one. Run `git worktree add` in a new terminal, do the fix there, push, and remove the worktree when the PR merges. The original branch never stops running.

**The sharp edge:** worktrees share `node_modules` by symlink if you set it up that way, but framework-generated config paths (`.nuxt/tsconfig.json`, for instance) may not resolve correctly in the worktree. Unit tests ran fine; component-level tests needed the primary tree. This is liveable but worth knowing about upfront.

## Pattern 2: parallel sessions coordinating via issue comments

The situation: two Claude Code sessions are running on the same repository at the same time — one doing content research for a new blog entry, the other fixing bugs for the same release. Both are reading and writing to the same repo, the same issue tracker, and potentially the same files.

On tdf26, this worked without conflict because the two sessions had disjoint work surfaces. The content session was on a feature branch touching markdown files. The bug-fix session was on separate fix branches touching Vue components and Python scripts. No shared files, no merge conflicts.

The coordination happened through the issue tracker. The content session ran a GPX verification of a town's position and posted its finding as a comment on the relevant issue. The bug-fix session read that comment and used it as the authoritative input for its fix. No live handoff, no shared scratch file, no chat channel. The issue was the medium.

**When to use it:** any time two sessions (or two people) need to share a finding without sharing a work surface. Write what you learn on the issue, not in a local file. The issue is durable, timestamped, and visible to anyone who looks.

**What makes it work:** the surfaces must actually be disjoint. If two sessions edit the same file on different branches, the merge will be the coordination mechanism whether you planned for it or not. Disjoint surfaces turn coordination from a blocking problem into a lightweight one.

## The shared principle

Both patterns arrange the work so that two streams do not touch the same surface. Worktrees make branches disjoint on disk. Issue comments make findings durable without touching the working tree. Coordination still happens — the issue comment *is* coordination — but it happens asynchronously and without blocking either stream. The discipline is not "avoid coordination" but "arrange the surfaces so that the coordination that remains is cheap."

In [OODA terms](/unfold/notes/2026-04-11-nested-ooda-loops/), this is a way to run two Act loops in parallel without either one's Observe step being contaminated by the other's in-progress changes. The disjoint surfaces are the isolation boundary.

## Related

- [Loops inside loops: OODA at multiple scales](./2026-04-11-nested-ooda-loops.md) — the nested OODA framing that this pattern instantiates
- [Personas versus subagents](./2026-04-10-personas-vs-subagents.md) — a related question about how two Claude Code instances relate to each other
