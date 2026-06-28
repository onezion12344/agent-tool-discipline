# 模型多想想，人类少操心

> Let the model think more, so humans worry less.

**Three-tier tool discipline for AI agents** — a structured framework compensating for the bounded rationality of LLMs.

---

## The Problem

LLMs suffer from a fundamental **Knowing-Doing Gap** (Cheng et al., UMD, 2026): their internal representations encode awareness that a tool is needed, yet this signal becomes orthogonal to execution intent in late layers — causing 26.5%–54.0% necessity-action mismatch.

In plain terms: **the model knows it should use a tool, but it doesn't.**

---

## The Solution: Three-Tier Discipline

| Tier | Skill | Guards Against | Question It Answers |
|:--|:--|:--|:--|
| **1** | `exhaustive-tool-review` | Knowing-Doing Gap | "Did I scan ALL available tools?" |
| **2** | `cross-verify-search` | Single-source bias | "Can I trust what I found?" |
| **3** | `fact-check` | Factual errors | "Is what I'm about to say correct?" |

```
┌─────────────────────────────────────────┐
│  Tier 1: EXHAUSTIVE TOOL REVIEW         │
│  Scan all tools before concluding        │
│  "What else could help me?"              │
├─────────────────────────────────────────┤
│  Tier 2: CROSS-VERIFY SEARCH            │
│  ≥2 independent sources                  │
│  "Is this source reliable?"              │
├─────────────────────────────────────────┤
│  Tier 3: SYSTEMATIC FACT-CHECK          │
│  SIFT → CoVe+FIRE → FABLE → Truth Sandwich │
│  "Are these claims verified?"            │
├─────────────────────────────────────────┤
│  🎯  HUMAN: Receive trustworthy output   │
│  Zero manual correction needed           │
└─────────────────────────────────────────┘
```

---

## Academic Foundation

| Finding | Paper | What It Means |
|:--|:--|:--|
| Knowing-Doing Gap | [Cheng et al., 2026](https://arxiv.org/abs/2605.14038) | Models internally know but don't act on tool necessity |
| Temporal Blindness | [Cheng et al., 2026](https://arxiv.org/abs/2510.23853) | Models ignore time passage in multi-turn tool decisions |
| Tool Selection Depth | [Repantis et al., Meta, 2026](https://arxiv.org/abs/2605.24660) | Adaptive depth improves tool-choice accuracy by 6pp |
| Context Engineering | [Anthropic, 2025](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) | LLMs have finite attention budgets; curate context tightly |
| Tool Description Engineering | [Adaline Labs, 2026](https://labs.adaline.ai/p/ai-agent-tool-calling-failures) | Tool descriptions are the #1 signal for selection, not system prompts |

---

## Skills

- [`skills/exhaustive-tool-review.md`](skills/exhaustive-tool-review.md) — Force-scan all tools before concluding
- [`skills/cross-verify-search.md`](skills/cross-verify-search.md) — Cross-verify every search result
- [`skills/fact-check.md`](skills/fact-check.md) — Four-round systematic fact verification

---

## Philosophy

> Human attention is precious. Model compute is cheap.
>
> Every extra tool call the model makes is one less correction the human has to issue.
>
> **Shift the cognitive burden from human → model.**

---

## License

MIT — [onezion12344](https://github.com/onezion12344)
