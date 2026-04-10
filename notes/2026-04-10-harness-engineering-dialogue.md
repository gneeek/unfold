---
layout: page
title: "Two Perspectives on Harness Engineering in TDF26"
date: 2026-04-10
status: published
format: dialogue
sources:
  - https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents
  - https://openai.com/index/harness-engineering/
  - https://github.com/openai/symphony
  - https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html
---

# Two Perspectives on Harness Engineering in TDF26

A dialogue between two voices — the maintainability reviewer (cold-eyed, file-anchored) and the original developer (protective, context-aware) — on whether the "harness engineering" patterns being written up by HumanLayer and OpenAI apply to this project, where they conflict, and what small-batch experiments are worth trying.

## The three sources, boiled down

- **HumanLayer — "Skill Issue"**: harness = the levers around the model (CLAUDE.md, MCP, skills, sub-agents, hooks, back-pressure). Philosophy: **bias toward shipping, throw away configs that don't earn their keep**. Specific claims: CLAUDE.md < 60 lines; sub-agents are for context firewalling, not role-play; verification hooks should be silent on success, loud on failure; prefer CLI + 6 examples over a full MCP server.
- **OpenAI — "Harness Engineering"**: 1M LOC, 1,500 PRs, 5 months, zero hand-written code. Philosophy: **"Humans steer, agents execute."** AGENTS.md is the table of contents, not the encyclopedia. Plans, design docs, and tech-debt tracker are checked-in, versioned, machine-checkable. Custom linters inject remediation instructions into their own error messages. A doc-gardening agent runs on a schedule to kill drift. Rigid layered architecture (Types→Config→Repo→Service→Runtime→UI) enforced mechanically.
- **openai/symphony**: a daemon that reads Linear, spawns per-issue git worktrees, runs Codex unattended, and lets the agent drive a ticket from `Todo` → `Merging`. In-repo `WORKFLOW.md` (YAML front matter + prompt body) is the policy. A "workpad" (single persistent Linear comment) is the source of truth for progress. Skills in `.codex/skills/<name>/SKILL.md` with frontmatter.

## Dialogue

---

**Reviewer** *(cold-eyed, file-anchored)*: Let me start with the uncomfortable read. The maintainability report I just finished found four hand-rolled frontmatter parsers, five copies of route metadata, and two divergent publish pipelines. OpenAI's answer to exactly this kind of drift is "custom linters with remediation instructions injected into the error messages." We have **one** validation script (`processing/validate_entries.py`) and it's itself one of the hand-rolled parsers. The gap between where we are and the OpenAI pattern isn't philosophical — it's that we haven't built the feedback loops yet.

**Developer** *(protective, context-aware)*: Fair, but let's be honest about what this project is. TDF26 is a **content site with a supporting codebase**, not a million-line product. "Humans steer, agents execute" lands differently when the product is prose. The one thing we most want to keep human is segment entry voice — that's the whole point. So I read the OpenAI post with a bifurcation: everything they say about the **code/data/publish** layer applies almost directly; everything about "agents write everything" doesn't apply to the content layer and shouldn't.

**Reviewer**: Agreed — and that bifurcation is *exactly* what the Fowler framing calls out. The "behaviour harness" is the weakest part even at OpenAI; where coding agents shine is the maintainability and architecture harnesses. Those are code concerns, and we have code concerns in spades.

**Developer**: Right. So let's separate the signal from the hype. What aligns with how we already work?

### Where we're already aligned

**Reviewer**: More than I expected, actually.

- **CLAUDE.md exists and gets loaded deterministically** — we already use the primary lever both posts name first.
- **Skills** — our setup has `commit`, `review-pr`, `update-config`, `schedule`, etc., which matches the progressive-disclosure pattern HumanLayer describes and the `.codex/skills/*` layout Symphony uses.
- **Sub-agents for context control** — the maintainability-reviewer run we did an hour ago is *the* HumanLayer pattern: spawn a sub-agent, it burns context on greps and file reads, it returns a condensed report with `file_path` citations, the parent never sees the intermediate noise. We're already doing this correctly.
- **Memory as versioned context** — the `memory/` system is a lighter, user-scoped version of OpenAI's "push everything into the repo so agents can see it" principle. Different scope, same instinct.
- **Validation scripts as back-pressure** — `processing/validate_entries.py`, `processing/rider_stats.py --data-cutoff`, `processing/snapshot_stats.py` are all back-pressure mechanisms. The bug fixed in 86881fa (dataCutoff injection) was literally a back-pressure gap being closed.
- **Short PRs, clear goals** — the durable preference (1-3 goals per release, PR per goal, feature branches → main) is *exactly* the short-lived-PR norm the OpenAI post describes, just at a much smaller throughput.

**Developer**: Good list. The "we're already doing most of this, just less formally" point is important. We shouldn't read these posts as "you're wrong about everything." We should read them as "here's vocabulary for what you already do, plus three or four concrete patterns to try."

### Where they're in direct conflict with how we work

**Reviewer**: The conflicts are real, though. Let me name them.

1. **CLAUDE.md size.** OpenAI: ~100 lines, table of contents. HumanLayer: under 60 lines. Ours: ~600 lines, with embedded research on Tour de France history, Wikimedia Commons categories, confirmed photographers, and historical topics per area. That's not a table of contents — it's an encyclopedia. Every conversation pays a context tax on content that 90% of sessions don't need.

2. **Plans are not first-class.** OpenAI treats `docs/exec-plans/active/` and `docs/exec-plans/completed/` as checked-in artifacts. We have the memory system and GitHub issues, but no in-repo versioned plan log. When a second publisher joins, they'll have to reconstruct "what was tried" from git history and memory files they don't have access to.

3. **Mechanical enforcement is thin.** The OpenAI "golden principles" pattern — opinionated rules encoded as lints, with error messages that tell the agent how to fix — would have caught at least four of the items in the maintainability report before they merged. The four-way frontmatter parser duplication? One lint. The divergent `scripts/publish.sh` vs `server/api/publish.post.ts` pipelines? One structural test.

4. **No doc-gardening.** OpenAI runs a recurring Codex task that scans for stale docs and opens fix-up PRs. We have a dead `/admin/schedule` link in `pages/admin/index.vue` and a `processing/README.md` that documents 5 of 11 scripts. That's classic drift the pattern is designed to catch.

**Developer**: And now the ones that are load-bearing disagreements.

5. **Symphony's full-autonomy posture.** The durable preference is "verify PR merges via GitHub API, don't rely solely on user reports" — we've been burned by agents claiming success when things weren't actually merged. Symphony goes further in the opposite direction: it moves tickets through `Human Review` → `Merging` → `Done` with the agent doing the merging via its own `land` skill. That posture doesn't match how we work, and it shouldn't, at this stage, with two humans and a public-facing content product.

6. **"No manually-written code" literalism.** OpenAI's discipline works because their product is software and their bottleneck was dev velocity. Our product is a twice-weekly narrative voice. The equivalent for us would be "no manually-written *code*, but all entry prose is human." That reframing is fine — it just needs to be explicit so nobody tries to generate segment 12 from a prompt.

7. **Throughput changes merge philosophy.** OpenAI can tolerate test flakes and rerun because they merge 10+ PRs/day. We merge ~2/week. Their "don't block on flakes" is genuinely not our situation. Small-batch + careful still wins here.

**Reviewer**: That last one is worth underlining. Several of OpenAI's most aggressive claims — minimal blocking gates, agent-merge-own-PRs, flake = rerun — are **throughput-dependent optimizations**. Copying them at our scale would import the risks without the offsetting benefit.

### Symphony specifically — what's Codex-locked vs. portable

**Developer**: Before we get to recommendations, let's close the loop on Symphony, since the question explicitly asked about Claude Code applicability.

**Reviewer**: The Codex-specific surface area is narrow:
- `codex --config ... app-server` command + JSON-RPC thread protocol (not portable; Claude Code has `claude -p` headless mode and the Agent SDK, different shape)
- `apply_patch` tool expectations
- `Codex Review — <persona>` review-comment convention on PRs
- `thread_sandbox: workspace-write` approval model
- GPT-5.3-Codex model pinning

What's **fully portable** to Claude Code:
- The **`WORKFLOW.md` front-matter-plus-prompt pattern** — put policy in-repo, version it with the code. Trivially adoptable.
- The **workpad pattern**: "single persistent comment is the source of truth for an issue; don't post N progress comments, edit one." We could adopt this for GitHub issues tomorrow.
- The **`.codex/skills/<name>/SKILL.md` format with YAML front matter** — identical to how Claude Code skills already work. The content of `commit/SKILL.md`, `pull/SKILL.md`, `land/SKILL.md` is 85% git/gh CLI operations and would port with minimal edits.
- The **state-machine routing** (`Todo` → `In Progress` → `Human Review` → `Merging` → `Rework` → `Done`) as a mental model for any kind of ticket, regardless of harness.
- The **"blocked-access escape hatch"** discipline: only escalate to humans for true blockers, document them in a structured brief, keep trying documented fallbacks first. That's a values statement, not a Codex feature.

**Developer**: The `.codex/skills/land/SKILL.md` in particular is a gold mine of specifics on PR review-reply mechanics (`in_reply_to` must be numeric, not the GraphQL node ID; use the `/pulls/<n>/comments` endpoint not issue comments, etc.). That kind of tribal knowledge is exactly the "things you only learn by getting burned" that the memory system is designed to capture. Worth harvesting even if we ignore the rest of Symphony.

### What to try, incrementally, in small batches

**Reviewer**: Let me propose experiments ranked by **cost-to-try** (low is good) vs **expected maintainability payoff**. All of these fit a single PR.

**Experiment 1 — Slim CLAUDE.md, move reference content to `docs/references/`.** *Cost: 2 hours. Payoff: every conversation gets cheaper context; reference content becomes discoverable on demand instead of always-loaded.* Split `CLAUDE.md` into: (a) a ~100-line map that names the four pillars, key file paths, and where to find everything else, and (b) `docs/references/tour-history.md`, `docs/references/image-sources.md`, `docs/references/known-climbs.md`. This directly implements the OpenAI "table of contents, not encyclopedia" pattern.

**Experiment 2 — Introduce `docs/exec-plans/active/` and `docs/exec-plans/completed/`.** *Cost: 1 hour to create, then ongoing discipline. Payoff: a second publisher has a place to read "what's in flight and why."* Retroactively convert the maintainability review output into `docs/exec-plans/active/2026-04-10-maintainability-baseline.md`. Everything future should land here before code is touched.

**Experiment 3 — One shared frontmatter parser + a lint that fails on hand-rolled ones.** *Cost: half a day. Payoff: kills four copies of the same logic AND establishes the "custom linter with remediation message" pattern.* Write it once in Python and once in TypeScript (the shared API surface is small). Then add a `validate_entries.py` check that greps for the banned regex patterns in `server/api/*.ts` and `processing/*.py`, with an error message that literally tells Claude "import the shared parser from X instead." Pure OpenAI pattern, but small-batch.

**Experiment 4 — A Claude Code Stop-hook for back-pressure.** *Cost: 2 hours. Payoff: silent-on-success verification after every Claude session.* Exactly the HumanLayer hook pattern: on Stop, run `python processing/validate_entries.py && npx nuxt typecheck`, silent on success, surface only errors, exit 2 to re-engage. This is the single highest-leverage item on the list.

**Experiment 5 — Workpad discipline for non-trivial GitHub issues.** *Cost: a shared convention, no code. Payoff: the incoming second publisher has one place to read per issue.* Adopt Symphony's rule: one pinned comment per issue, edited in place, with Plan / Acceptance Criteria / Validation / Notes sections. No new tooling needed.

**Experiment 6 — A scheduled doc-gardening run.** *Cost: 1 hour to set up via the `schedule` skill. Payoff: catches drift before it rots.* Weekly: Claude Code runs a small prompt that checks for dead links in `pages/*`, stale references in `CLAUDE.md` to files that have moved, TODOs older than 30 days, and discrepancies between `processing/README.md` and the actual script list. Output a single GitHub issue if anything is found. Kill-switch if it gets noisy.

**Developer**: I'd add one more, and it's the one that feels most **this-project-specific**:

**Experiment 7 — Publish pipeline as the first place you write a "golden principles" doc.** *Cost: half a day. Payoff: the divergent `scripts/publish.sh` vs `server/api/publish.post.ts` problem gets solved structurally, and the new publisher has a single page to read.* Not "refactor the pipeline to be one thing" — that's a bigger bet. Just: write `docs/publish/principles.md` that enumerates what MUST be true for a publish to happen (validate entries, regenerate stats with matching `--data-cutoff`, regenerate points, inject weather, generate site, deploy). Then reference it from both the CLI and the API path. The divergence becomes visible the moment either path skips a principle. This is the OpenAI "enforce invariants, not implementations" pattern at toy scale.

**Reviewer**: I like that one because it addresses the highest-ranked item in the maintainability report without requiring a big refactor. It's pure scaffolding — exactly what both posts argue matters most.

### What to reject (or at least not adopt yet)

**Reviewer**:
- **Full Symphony deployment.** It's designed for trusted-environment, high-throughput, near-autonomous operation. We are none of those things. The **patterns inside** Symphony are worth harvesting; the daemon itself is not a fit.
- **Auto-merging agent PRs.** Directly conflicts with the PR-merge-verification preference. Don't even experiment.
- **Per-worktree ephemeral observability stack.** Massive overkill for a static blog. Worth remembering *exists* if TDF26 ever grows a backend with real telemetry.
- **"Boring tech, sometimes reimplement."** OpenAI wrote their own p-limit clone for agent-legibility reasons. We are a small Nuxt site; normal library usage is fine.
- **1,500-PR merge philosophy.** Stay in "careful, small batches" mode. Our throughput doesn't unlock their tradeoffs.

**Developer**: And one philosophical reject from me:

- **The "humans steer, agents execute" frame as applied to entry prose.** For `segment-NN.md` content, the agent is a research assistant and a proofreader — not an author. That matches the existing content-workflow preference (research-then-discuss-then-draft). Don't let the OpenAI post erode that. The harness patterns apply to everything *around* the entries, not the entries themselves.

### The one-sentence summary

**Reviewer**: Most of the harness-engineering playbook applies to TDF26's code layer directly, at small-batch scale, without adopting the autonomous-agent posture.

**Developer**: And the content layer should borrow the *context discipline* (short CLAUDE.md, references on demand, exec-plans as artifacts) without borrowing the *authorship posture*.

---

## Concrete next-step recommendation

If we pick *one* thing to try first, the **Stop-hook for back-pressure (Experiment 4)** has the best cost/payoff ratio. It's 2 hours, touches no existing code, silently improves every future Claude session, and is the single most-cited technique across all three sources. Experiment 1 (slim CLAUDE.md) is a close second and can be done in the same sitting.

---

## Second pass — the publisher pushes back

The first dialogue above was saved as a metablog candidate. Before it got filed away, the publisher read it back and pushed on two things. What follows is a second pass that re-opens the conversation with a third voice — the publisher's — bringing the corrections into the room.

**Publisher**: I have a couple comments about the harness engineering report. First: in this project, my intention has been to store good-practice skills and "how we work" memories, along with the project roadmap plan, in the wiki that sits alongside the code repo. It seems to me like that did not get included in the comparison. Can you have both voices comment on that. Second: I like the distinction between code and content, but I'd add that in this project the agent has been doing a first draft of the content, guided by the human, with interaction. So it is not completely accurate to say "human-authored content." I'd also like both voices' opinions on this point.

### Correction 1 — the wiki is the system of record I claimed we were missing

**Reviewer**: I have to concede this one cleanly. I wrote "plans are not first-class" and "no in-repo versioned plan log" — and then the publisher pointed me at the wiki, which has exactly that. `How-We-Work.md` is already the "how we work" doc. `Roadmap.md` already has a Now/Next/Later/Ongoing/Completed structure, with the explicit rule that "goals are sentences, not issue titles." The retrospectives (`Retro-v1.3.1-v1.3.4.md` and friends) are exactly the versioned decision log OpenAI describes in their design-docs directory. Functionally this is the same pattern I said was missing.

I'll go further: `Claude-Code-Configuration.md` already notes that `CLAUDE.md` is 542 lines and *planned* for refactoring via #309 into a portable `.agent/` directory in v1.4.1. My "Experiment 1" in the first pass was essentially proposing something that's already on the roadmap. I was reinventing a ticket that exists.

**Developer**: Right. And the mapping is closer than you admit:

| OpenAI pattern | TDF26 equivalent |
|---|---|
| `AGENTS.md` as table of contents | `CLAUDE.md` (currently over-long, v1.4.1 will slim it) |
| `docs/design-docs/index.md` + core-beliefs | Wiki `Home.md` philosophy section + `How-We-Work.md` principles |
| `docs/exec-plans/active/` | Wiki `Roadmap.md` Now + Next sections |
| `docs/exec-plans/completed/` | Wiki `Roadmap.md` Completed section + retrospective pages |
| `docs/product-specs/` | `CLAUDE.md` pillars section + wiki Home |
| Core beliefs | Wiki `How-We-Work.md` Goals (five of them, ordered) |
| Tech-debt tracker | GitHub issues with `goal:` labels, plus the retrospective "what we lack" sections |

**Reviewer**: So the correction isn't "the wiki exists and I missed it" — it's "the wiki is *doing the job* and I missed that." The interesting question becomes: what's different about putting it in a GitHub wiki instead of `docs/` in the main repo? That's a real design choice, not an accident.

**Developer**: The choice has upsides that matter:

- **Editorial voice is cleaner.** Wiki edits don't clutter code PRs. The retro for v1.3.1-v1.3.4 doesn't need to be reviewed as if it were code.
- **Non-developers can contribute.** A second publisher can edit How-We-Work in a browser without a git workflow.
- **It's browsable.** Someone landing on the repo sees Home → How-We-Work → Roadmap as a readable hierarchy, not a `docs/` tree buried in the file list.
- **It's linkable from issues.** `[How We Work](../../wiki/How-We-Work)` as a stable reference.

**Reviewer**: And the downsides, which are what I was reacting to without knowing it:

- **Claude Code doesn't see the wiki automatically.** The wiki lives at `gneeek/tdf26.wiki.git`, a separate repo. When a conversation starts in `/home/jhs/code/tdf26`, the wiki content is not in context. CLAUDE.md is. Memory is. But the wiki — which is the actual source of truth for "how we work" — is invisible unless the agent is told to clone it or the user pastes content in.
- **That forces duplication.** `CLAUDE.md` restates a lot of what `How-We-Work.md` already says (the five goals, the PR workflow, the content workflow, the emdash rule). Memory restates more of it (gitflow, roadmap style, work loops, content workflow). Same facts, three places, three update cadences. Drift is baked in.
- **Mechanical enforcement is hard across repo boundaries.** OpenAI's "custom lints that cite docs" trick works because docs and code are in one tree. A lint in this repo can reference `wiki/How-We-Work.md#publishing` by URL, but can't *verify* the URL still resolves or that the section still says what the code expects.

**Developer**: So the legibility gap is real, and the #309 `.agent/` plan is the current answer. But we should re-scope #309 now that we're looking at it through this lens. Two options:

- **Option A — `.agent/` mirrors wiki content.** Small stub files in `.agent/` that say "the authoritative source for X is wiki/How-We-Work.md#section-name, here's a 5-line summary for quick context, see wiki for the full version." The wiki stays canonical; Claude gets a cheap in-repo pointer. A pre-publish or nightly check confirms the wiki pages referenced still exist.
- **Option B — `.agent/` *is* the content, wiki generates from it.** Keep prose authoritative in `.agent/*.md`, and a sync step pushes to the wiki (or the wiki becomes a generated mirror). Single source of truth, agent-native, but you lose the "edit in browser" benefit.

**Reviewer**: Option A is smaller-batch and doesn't force an editing-workflow change on the second publisher. Start there. Option B is the answer if the duplication becomes painful after a few release cycles.

**Developer**: One more thing the wiki surfaced: the memory system's `MEMORY.md` index doesn't mention the wiki at all. It points to user/feedback/project/reference memories but never says "also, the authoritative source for how we work is the wiki." That's a tiny fix with real payoff — add one line pointing agents at the wiki URL, note that it's a separate repo, and tell them when to clone it. That's a free fix independent of #309.

**Reviewer**: Agreed. And that's the first concrete revision to the first-pass dialogue: **Experiment 0 — tell the memory system the wiki exists.**

### Correction 2 — "human-authored content" was the wrong frame

**Developer**: The publisher is right that I overstated this. I wrote things like "the one thing we most want to keep human is segment entry voice" and "don't let the OpenAI post erode that" — which implied a hands-off-for-content posture that doesn't match reality. `How-We-Work.md` actually says it clearly:

> For blog entry content, follow this workflow:
> 1. **Research** — Explore 4-6 topic areas specific to the segment
> 2. **Discuss** — Present findings as bullet points, decide what to emphasize or drop
> 3. **Draft** — Write the entry based on agreed topics
> 4. **Review** — Check on the dev server before publishing

The draft is agent-written. The memory system literally has an entry called "Content workflow — research-then-discuss-then-draft" and I still reached for the "human authorship" shorthand.

**Reviewer**: The shorthand matters because it changes what the harness-engineering playbook says about content. If content is agent-drafted from a negotiated brief, then content is exactly the kind of workload harness patterns apply to. Back-pressure, skills, sub-agents, context discipline — all of it lands. The "humans steer, agents execute" framing from OpenAI isn't violated by this project at all; it's the actual workflow.

**Developer**: So what IS the thing I was trying to protect? Not keyboard-fingers. What I actually want to preserve is:

- **Human judgement about what matters in a given segment.** Why this hilltop and not that one. Why Turenne's viscount genealogy is worth 80 words and the geology gets 40.
- **Human judgement about what to drop.** The research phase always surfaces more than fits; the Discuss step is where the filter lives.
- **Human judgement about how this entry connects to other entries.** Foreshadowing from segment 1 that needs to land in segment 4. Echoes from a retrospective. Tone consistency across the full arc of 26.
- **Human judgement about when a draft isn't working and needs to be thrown away.** Not "it has a typo" — "this framing is wrong, start over."

That's all at the **Discuss** and **Review** steps. The Research and Draft steps are the agent's. And the harness-engineering pattern says: invest in scaffolding for Research and Draft so Discuss and Review are the places the human actually spends time.

**Reviewer**: Which unlocks a whole class of experiments I should have proposed in the first pass. Content-side harness investments:

- **Research skill as a sub-agent pattern.** A sub-agent that, given a segment, returns a condensed research report with `source_url:section` citations for every factual claim. Parent context never sees the 40 Wikipedia pages. This is the exact HumanLayer pattern applied to content instead of code.
- **Content validation as custom lint.** Run on a draft: checks for emdashes (per convention), checks for specific claims (dates, statistics, named people) that lack a corresponding citation in the frontmatter `sources` field, checks for voice drift by comparing n-gram distributions against previously shipped entries. Error messages tell Claude how to fix: "Line 47 says '99 men hanged' — add a source to frontmatter `sources:` or soften the claim."
- **Draft review sub-agent.** After a draft is written, spawn a reviewer sub-agent with a prompt like "you are a travelogue editor. This is segment N of 26. Prior segments are summarized here. Flag: claims lacking citation, factual uncertainty, voice drift from prior entries, promises from earlier segments that this one should deliver on, passages where the author is telling instead of showing." Returns a structured critique. Human reads the critique during Review step.
- **Foreshadowing tracker.** The Roadmap Ongoing section already tracks this informally ("Foreshadowed content from segment 1... tracked as entries are written"). A checked-in `data/foreshadowing.json` with a lint that errors if a segment contains a known-foreshadowed topic without mentioning it, or claims to introduce a topic that was already introduced earlier. Mechanical version of something currently in human heads.

**Developer**: Every one of those is more valuable than most of the code-side experiments I proposed. Because content quality is goal #2 and developer experience is goal #4 in the wiki's Goals list. I was proposing experiments against the wrong goal.

**Reviewer**: That's the real correction. Rereading the first pass, my experiments 1-7 are all pointed at maintainability and developer experience — which is fine, I'm the maintainability reviewer, that's my lane. But the bigger leverage for *this project's stated goals* is content-side harness work.

**Developer**: One more subtlety. The Discuss step is the one thing in that Research→Discuss→Draft→Review loop that should **not** be harnessed away. It's the load-bearing human-judgement step. A harness that tries to automate the Discuss step by having an agent "choose what to emphasize" would be optimizing exactly the thing we don't want optimized. So the rule is: harness everything around Discuss; leave Discuss alone.

**Reviewer**: That's a principle worth writing down. "Harness the loops, not the judgement." Could be a line in the `.agent/` refactor, or a wiki addition.

### Net effect on the first-pass dialogue

**Developer**: If we were to revise the first pass based on these two corrections, the changes would be:

1. **Kill the framing that the wiki is missing.** It exists and it's doing the job. The real issue is that it's illegible to Claude Code without an explicit bridge.
2. **Add Experiment 0**: tell `MEMORY.md` the wiki exists. One-line fix, free win.
3. **Re-scope Experiment 1**: don't propose a `docs/references/` tree; the refactor is already tracked as #309 targeting `.agent/`. Instead propose how `.agent/` should relate to the wiki (Option A: stubs + links).
4. **Kill Experiment 2 ("exec-plans/ directory").** The wiki Roadmap + retrospectives already are this. Duplication.
5. **Revise the "code vs content" framing.** Replace "don't let the OpenAI post erode content being human-authored" with "harness everything around the Discuss step; the Discuss step itself stays human."
6. **Add Experiments 8-11 on the content side**: research sub-agent, content lints, draft reviewer sub-agent, foreshadowing tracker.
7. **Reorder the recommendations.** The single highest-leverage experiment is no longer the Stop-hook for back-pressure — it's the draft reviewer sub-agent, because it serves the top-priority goals (reader experience + content quality) rather than goal #4 (developer experience).

**Reviewer**: Slight pushback on point 7. The Stop-hook for back-pressure is still the lowest-cost, fastest-to-implement experiment, and it serves the code side which *also* serves publisher experience (goal #3). Better framing: **content-side experiments have higher ceiling; code-side experiments have faster time-to-value.** Do both, and pick based on which goal is hurting most in the next release cycle.

**Developer**: Last note: the #309 refactor is landing in v1.4.1 per the roadmap, which is "Next" right after v1.4.0. So the wiki-to-`.agent/` bridge question is about to become a live design decision anyway. Good timing for this dialogue to exist.

---

## What the two passes together suggest

- The first pass was a stranger reading a new codebase. It found real patterns, and it also hallucinated a gap (the wiki) because it was reading from the code tree alone.
- The second pass happened because the publisher did the one thing the first pass most needed: it refused to let the report stand without pushing on the framing.
- That loop — draft, push back, revise in dialogue — is itself a harness. It's the Discuss step applied to process writing instead of segment prose. And it's the reason the second pass is better than the first.
- Concrete revised order of things to try:
  - **0.** Add a wiki pointer line to `MEMORY.md` index (free, minutes).
  - **1.** Re-scope #309 (`.agent/` refactor) to explicitly decide the wiki relationship (Option A vs B). Due anyway in v1.4.1.
  - **2.** Stop-hook for back-pressure (code side, fastest time-to-value).
  - **3.** Draft reviewer sub-agent (content side, highest ceiling).
  - **4.** Content lints for emdashes and uncited specific claims.
  - **5.** Publish pipeline principles doc (previously Experiment 7).
  - Everything else deferred or watched.
