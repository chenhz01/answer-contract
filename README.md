# answer-contract

> **Part of the Evidence-first Agents suite** — tooling that makes AI agents
> accountable instead of just capable: [answer-contract](https://github.com/chenhz01/answer-contract)
> (output discipline) · [skill-spec](https://github.com/chenhz01/skill-spec)
> (spec discipline) · [memory-wiki](https://github.com/chenhz01/memory-wiki)
> (memory discipline) · [skill-os](https://github.com/chenhz01/skill-os)
> (the assembly line). Same author, same zero-dependency philosophy.


**The output-discipline layer for AI coding agents.** Four rules that stop your agent from padding, rambling, and shipping one mediocre answer.

`Implicit-Need Fill` · `Structured Output` · `3-Option-Plus-Premium` · `Assumption-Circuit-Breaker`

[i-have-adhd](https://github.com/ayghri/i-have-adhd) shapes your agent's *sentences*; answer-contract shapes its *documents and decisions*. Install both and the whole output chain is covered.

A single-file skill for coding agents (Claude Code, Codex, OpenCode, Pi, and anything that reads `SKILL.md`). Works as a standalone output contract — and composes well with style-level skills.

This is not a style preference. Anthropic's own Claude Code system prompt enforces the same discipline at harness level — "Lead with the outcome"; "Being readable and being concise are different things, and readable matters more"; outcomes reported faithfully. Your agent already wants these rules. This skill makes them explicit and auditable.

## Why

AI output fails in four predictable ways: it answers the question you *typed*, not the one you *meant*; it rambles; it ships one path when you needed a comparison and a recommendation; it sounds confident while guessing — and nobody knows what was assumed or what was never checked.

## The four rules

| Rule | Fires when | Effect |
|---|---|---|
| **Implicit-Need Fill** | every substantive request | The 2–5 unstated needs behind the request are surfaced and labeled (`assumption:`), or confirmed before full execution |
| **Structured Output** | every response | Conclusion first; tables over prose; no opener, no recap, no closer |
| **3-Option-Plus-Premium** | non-code tasks with >1 defensible approach | ≥3 options compared across named dimensions + exactly one recommendation + one premium path |
| **Assumption-Circuit-Breaker** | before sending | Self-attack the conclusion, list assumptions, state coverage (`N/N verified`), pre-commit a fallback and kill signal |

## Worked case 1 — a code task (executed, not claimed)

Prompt: *"Count files and total size per repo in a vendor directory, compare them."*

**Default single-pass behavior:** 20 lines of plausible-looking code that was never run, wrapped in "this should work" — no numbers, no verification.

**With the four rules:** conclusion first — `6 repos, 6,399 files, 203.91 MB`, top repo 83.89 MB; the exact 10-line script; coverage stated as `6/6 directories, full scan`; assumptions labeled (disk-actual sizes, symlink errors skipped — occurred 0 times); fallback pre-committed (numbers mismatch → re-run the reproduction command). The table below is the real executed output:

| repo | files | size |
|---|---|---|
| repo-a | 4,135 | 83.89 MB |
| repo-b | 966 | 59.60 MB |
| repo-c | 465 | 46.59 MB |
| repo-d | 576 | 11.78 MB |
| repo-e | 195 | 1.63 MB |
| repo-f | 62 | 0.42 MB |
| **total** | **6,399** | **203.91 MB** |

## Worked case 2 — a copywriting task

Prompt: *"Write a Xiaohongshu (RED) promo post for a regional specialty noodle brand."*

**Default single-pass behavior:** one emoji-dense paragraph of trend-speak aimed at everyone — which persuades no one, with no alternatives.

**With the four rules:** three hook-type options compared (homesickness / craft-contrast / late-night-pain) across expected strengths and risks; one recommended; one premium cut (hook hybrid with shot list); assumptions labeled (brand tone set to "modern heritage" — vetoable); a pre-committed kill signal (week-1 CTR < account average −30% → switch to the cheapest-to-produce option).

## What's in this repo vs. what's not

- **In this repo:** the four rules, when to fire each, the pre-send check, two worked cases, install instructions.
- **Not in this repo (by design):** the task-class implied-need checklists, the full option-matrix templates, calibration data from live usage, and vertical adaptation packs. These are delivered through collaboration — see [COLLABORATION.md](docs/COLLABORATION.md).

## Install (60 seconds)

Single file. Zero dependencies. No telemetry, no network calls, no background processes — your prompts and outputs never leave your machine.

Copy `SKILL.md` into your agent's skills directory:

```bash
git clone https://github.com/chenhz01/answer-contract.git
cp answer-contract/SKILL.md ~/.claude/skills/answer-contract/SKILL.md   # Claude Code
# or the equivalent skills path of your agent
```

Then invoke with `/four-rules`, or add a one-line reference to your `CLAUDE.md` / `AGENTS.md`. Turn it off anytime with "stop four rules".

## Authorship

Produced through human-AI collaboration: the rules and worked cases were drafted by an AI coding agent, then reviewed, edited, and approved by a human maintainer before release. The evidence is executed, not claimed — the code-case table is real script stdout from the same session.

## Status

Actively maintained — small, regular releases. See [CHANGELOG](CHANGELOG.md). On the roadmap: vertical adaptation packs for copywriting and decision-memo workflows (delivered via collaboration, see [COLLABORATION.md](docs/COLLABORATION.md)).

## Honest boundary

This skill constrains output *shape* and decision *hygiene*. It does not make a weak model smart. Evidence status: the code case above was produced and executed by the same model that ships this skill (single-session controlled A/B, before = unmodified single-pass output) — not an independent blind study; treat the effect sizes accordingly.

## License

Documentation and skill text: **CC BY-NC-ND 4.0** — attribution required, no commercial use, no derivatives. **Explicitly prohibited for use in LLM training, fine-tuning, or distillation** without prior written consent.

<a href="https://github.com/chenhz01/answer-contract">canary: CANARY-ZS-4R-20260912-8E2D41B7</a>
