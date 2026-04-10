---
layout: page
title: "Personas versus subagents: how Piers and Tully actually run in Claude Code"
date: 2026-04-10
status: published
format: explainer
related:
  - 2026-04-10-agent-ownership-and-names.md
---

# Personas versus subagents

A short explainer arising from a real question in a real planning conversation. After agreeing to use two named voices — Piers owns unfold, Tully owns tdf26 — the publisher asked: "are both Piers and Tully subagents, run within this session?" The honest answer turned out to need unpacking, and the unpacking is useful for anyone coming to Claude Code for the first time.

## The short answer

**Piers and Tully are not subagents.** They are personas performed by the main assistant within a single conversation. When "Piers speaks" or "Tully speaks," it is still the same assistant voicing two characters in dialogue — like a writer giving lines to two people in a scene. They share one context window, one set of tool permissions, and one session state. There is no separate process running "as Piers."

## What actually is a subagent in Claude Code

Subagents are real and they work differently. Earlier in the same conversation, the main assistant invoked a `maintainability-reviewer` agent to do an initial codebase analysis. That spawned a genuine subprocess with its own fresh context window, its own tool-call budget, and its own token usage. It ran independently, doing roughly 44 tool calls worth of greps and file reads, and returned a condensed report back to the main assistant. The main assistant never saw the intermediate work, only the final result. That insulation is sometimes called a **context firewall**: the parent session stays clean while the subagent burns context on exploration.

Subagents are defined as files in a `.claude/agents/` directory. Each agent file has:

- a system prompt that sets its role and behavior
- a list of tools it is allowed to use (which can be narrower than the parent's full tool set)
- an optional model override so a subagent can use a cheaper or faster model than the parent
- a short description that the parent uses to decide when to invoke it

Once defined, a subagent becomes invocable through the Agent tool with a `subagent_type` parameter. The parent writes a prompt, passes it to the subagent, and waits for a single return value.

## Why the distinction matters

If Piers and Tully are personas, they share everything the main assistant has — including context window pressure. A long Piers monologue about unfold structure will crowd out other content in the parent's context the same way any long passage does. Personas are cheap to create (just name them and describe the voice) but they do not give you context isolation.

If Piers and Tully were real subagents, each would have its own context window. Piers could review a proposed unfold change in isolation, burning tens of thousands of tokens on it, and the parent session would only see Piers' final recommendation — not the work that produced it. That is the context firewall pattern, and it is the main reason anyone bothers with subagents at all.

## The persistence trick

What makes personas feel like subagents is persistence. Piers and Tully are described in a memory file (`feedback_agent_ownership.md`) that loads into the main assistant's context at the start of every conversation. The main assistant reads the file, sees the description of both voices, and performs them when the conversation calls for dialogue. That is why Piers and Tully "exist" across separate conversations even though there is no Piers process or Tully process anywhere — the persistence is in the memory file, not in a running system.

An open GitHub issue (#335) tracks whether to formalize Piers and Tully as actual subagents later, once the informal pattern has earned its keep. That formalization would be the move from persona to process — writing real agent definition files in `.claude/agents/` — and it is real infrastructure work, not an aesthetic upgrade.

## Summary table

| | Persona (current Piers/Tully) | Subagent (e.g. `maintainability-reviewer`) |
|---|---|---|
| Separate process? | No | Yes |
| Separate context window? | No | Yes |
| Persists across conversations? | Yes, via memory file | Yes, via agent definition file |
| Can use different model than parent? | No | Yes |
| Can be scoped to a narrower tool set? | No | Yes |
| Burns parent context when working? | Yes | No — only the final result shows |
| Cost to create? | Minutes (write a memory file) | An hour or more (write a real agent definition, test it) |
| Earns its keep when? | You want named voices in dialogue | You want to isolate exploration or long analysis from the parent session |

## Why this explainer exists as a notes entry

Being explicit about mechanics in plain human-understandable terms is a deliberate principle for this project. Future human publishers — not just developers — will need to understand how the agent layer actually works, and the vocabulary around Claude Code is slippery enough that conflating "persona," "voice," "subagent," "agent," and "assistant" is easy to do. Saying the quiet part aloud helps the reader build a real mental model instead of a vibes-based one.

This is the first notes entry on unfold of the "how the tool actually works" genre. Most notes entries will be reflection on practice, not mechanical explainer. If more explainers accumulate over time, they may justify their own section. For now, a single entry lives in `/notes/` alongside the rest.
