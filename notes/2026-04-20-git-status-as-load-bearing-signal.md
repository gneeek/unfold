---
layout: page
title: "Keeping `git status` load-bearing"
date: 2026-04-20
status: published
format: summary
related:
  - 2026-04-20-hygiene-and-retrospective.md
tags:
  - git
  - hygiene
---

# Keeping `git status` load-bearing

`git status` is meant to be a load-bearing signal — at-a-glance confirmation that the working tree is in a known state, with no stray modifications and no forgotten work. When the signal becomes noisy, it stops being load-bearing, and a real modification buried in the noise will be missed.

The failure mode is gradual. A screenshot saved into the repo. A local scratch file. A build artifact. A `dist/` directory. Each one individually harmless; cumulatively, they turn `git status` into a wall of output the operator has learned to scroll past. When something genuinely unexpected appears — a 55-line modification to a file the operator does not remember touching — it sits in the same visual space as the noise, and the operator's attention routes around it.

The mechanism is not "be more careful when reading `git status`." Attention cannot fix a signal-to-noise problem. The mechanism is *keep the signal clean so carelessness is survivable.*

## The two-bucket rule

Every untracked file should fall into one of two buckets:

1. **Will be committed soon** — tracked and committed before `git status` becomes permanent clutter.
2. **Is local-only by design** — added to `.gitignore` so it never appears in `git status` at all.

There is no third bucket. The grey-zone "stuff I pasted into the repo and forgot about" is exactly where real modifications hide. A file that has been sitting untracked for longer than a few sessions is one of the two categories masquerading as ambiguous: either it should be committed (track it) or it should not be there at all (ignore it, or delete it).

`.gitignore` curation is therefore not a one-time setup task. It is a recurring hygiene activity. Each time an untracked file appears that is not going to be committed soon, it either gets deleted or added to `.gitignore`. That single discipline keeps `git status` trustworthy.

## Worked example

On tdf26, a small project that publishes reflective entries on a cycling tour, a 55-line diff to a published content file sat uncommitted in the working tree for four days. Nobody noticed. The working tree also held a dozen items of untracked noise — screenshots, local scratch files, a `dist/` build output directory, a terminal screenshot someone had dragged in. Each daily `git status` had rendered the real modification and the noise in the same visual block; the operator had been scrolling past the block for months.

The modification was the only local copy of a change that had been live in production through the built output. Any routine branch switch, stash, or cleanup run during those four days would have taken it. The near-miss held only because nothing destructive happened to touch it.

The mechanism that had decayed was not attention. The signal had been noisy long enough that the operator had trained themselves to ignore the noisy parts, and the modification had appeared in the same class of noise. The fix was not better reading — it was adding the dozen noisy items to `.gitignore` (or deleting them) so the next unexpected item in `git status` has nowhere to hide.

## Warning

This signal decays gradually. A single stray file does not degrade `git status` noticeably; a dozen do. Each individual addition feels too small to worry about. The first time the operator notices the decay is usually after it has caused a problem — and by then the habit of scrolling past is established, and the fix has to undo the habit as well as the backlog.

A useful frame: treat `git status` as equipment that requires maintenance, not as a command that always works. The maintenance is mechanical and fast — a line or two in `.gitignore`, or a deletion — but it has to happen often enough that the signal never drifts.

## Related

- [Hygiene and retrospective: different mechanisms](./2026-04-20-hygiene-and-retrospective.md) — why `.gitignore` curation is a hygiene task rather than a learning task, and why reflection alone cannot fix a signal-to-noise problem
