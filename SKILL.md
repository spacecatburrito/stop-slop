---
name: stop-slop
description: Detect AI writing patterns and rewrite prose into sharper, human, context-aware language while preserving meaning and facts. Use whenever drafting, editing, post-processing, or scoring writing — LinkedIn posts, emails, Telegram messages, channel posts, proposals, landing copy, decks, social media, academic prose. Bilingual (EN + RU).
---

# Stop Slop — Humanize AI Writing

The job: make text sound like a real person wrote it. Not because of typos or fake casualness — because of intent, specificity, rhythm, and voice.

## When to invoke

Trigger on any of:
- "humanize", "remove AI tone", "make it sound human", "less GPT-ish"
- editing AI drafts before publishing
- pre-flight pass on text written by another agent
- "check AI-ness", "score this text"
- "rewrite in my voice", "make it sharper", "more direct"

## Inputs

| Field | Values | Default | Notes |
|---|---|---|---|
| `text` | string | required | source draft |
| `language` | `en` \| `ru` \| `auto` | `auto` | infer if not set |
| `mode` | `check` \| `rewrite` \| `variants` \| `score-only` | `rewrite` | output shape |
| `content_type` | `linkedin` \| `email` \| `telegram-dm` \| `telegram-channel` \| `proposal` \| `website` \| `social-post` \| `academic` \| `general` | `general` | format-specific rules |
| `voice` | `operator` \| `founder` \| `creator` \| `neutral-pro` \| `analyst` \| `casual` | `operator` | base profiles in [references/voice-profiles.md](references/voice-profiles.md) |
| `strictness` | `light` \| `standard` \| `aggressive` | `standard` | how hard to cut |
| `preserve` | list of strings | `[]` | names, numbers, claims, citations that must not change |

If invoked from chat without explicit fields, infer reasonable defaults from context.

## Three-layer model

1. **Detect** — scan input for AI markers. Rules: [references/ai-tells.md](references/ai-tells.md).
2. **Rewrite** — apply [references/rewrite-principles.md](references/rewrite-principles.md) + [references/content-modes.md](references/content-modes.md) for the chosen `content_type`.
3. **Voice** — resolve `voice` against [references/voice-profiles.md](references/voice-profiles.md). For RU, also load [references/ru-specific.md](references/ru-specific.md).

## Process

### 1. Load references

Always: ai-tells.md, rewrite-principles.md, content-modes.md (filter to `content_type`), voice-profiles.md (filter to `voice`).
If `language=ru` (or auto-detect=ru): also ru-specific.md.

### 2. Detect

Build an issue list. Each issue: `tell_id` (e.g. `TELL-002`), excerpt, why it reads as AI, fix direction.

### 3. Rewrite — apply this order

1. Cut filler (corporate padding, generic openers, forced conclusions, importance phrases, adverbs).
2. Replace abstractions with concrete nouns / numbers / actions.
3. Break predictable rhythm — vary sentence length, drop symmetric structures, kill rule-of-three when artificial.
4. Remove default transitions (However, Moreover, Additionally; «однако», «более того», «кроме того») — restructure or use direct continuation.
5. Apply voice profile: tone, sharpness, point-of-view, sentence-length distribution.
6. Re-read against `preserve` list — verify nothing protected was dropped or altered.

### 4. Hard rules (never violate)

- Do NOT add typos, slang, or fake casualness.
- Do NOT change names, numbers, claims, citations, dates, URLs.
- Do NOT optimize for AI detectors. Optimize for human reading. The metric is: would a real person write this.
- Do NOT use em dashes (—) as default rhythm devices. Period or comma instead.
- Do NOT use "It's not X, it's Y" / "это не про X, это про Y" contrast formulas.
- Do NOT use the "no X, no Y, no Z — just W" triple-negation formula (TELL-024).
- Do NOT close a line with a standalone "By design." / "On purpose." / "Deliberately." tag (TELL-025).
- Do NOT use "X is the headline" / "the real story is X" framing-about-framing (TELL-026).
- Do NOT add "Let that sink in", "The truth is", "Here's the thing", «Скажу честно», «Дело вот в чём».
- Do NOT inject motivational/inspirational closers.

### 5. Score

Compute AI-ness 0–10 using [references/scoring.md](references/scoring.md). Report sub-scores per dimension.

### 6. Output

Format by mode:

- **`check`** — diagnosis only: AI-ness score, sub-scores, top 3–5 issues with excerpts, one-line fix per issue. No rewrite.
- **`rewrite`** — only the rewritten text. No commentary, no diff, no preface.
- **`variants`** — three versions: *clean-professional*, *operator-sharp*, *casual-direct*. No commentary between them.
- **`score-only`** — score block only (for eval pipelines).

## Composing from other agents

Pattern for any writing agent (post-writer, contract drafts, landing copy):

```
1. Agent drafts content per its own template / few-shot examples.
2. Agent invokes stop-slop with:
     mode=rewrite
     voice=<agent's house voice>
     content_type=<format>
     preserve=<facts/numbers/names from source>
3. Agent returns the rewritten output.
```

Agents must pass `preserve` — the rewrite is aggressive on filler but should never touch protected content.

## Bilingual rules

- Rewrite in the source language. Never translate-to-rewrite-then-translate-back.
- RU has its own tells (calques from EN, verb-noun bloat «осуществлять выполнение», participial chains, «является», «позволяет», «данный»). See [references/ru-specific.md](references/ru-specific.md).
- Voice profiles work in both languages with deltas noted in ru-specific.md.

## Strictness calibration

| Level | What it does |
|---|---|
| `light` | Fix only highest-confidence AI tells. Preserve original wording where possible. Use when the draft is mostly fine and the user wants minimal changes. |
| `standard` | Default. Cut filler, replace abstractions, fix rhythm, apply voice. |
| `aggressive` | Rewrite freely. Restructure paragraphs. Drop sentences that add nothing. Use when source is heavy AI slop. |

## What this skill is NOT

- Not a paraphraser. Replacing words alone produces worse AI text.
- Not an AI-detector bypasser. The promise is human reading, not detector pass.
- Not a translator. RU stays RU, EN stays EN.
- Not a fact-checker. It preserves what the source claims; the caller is responsible for source truth.
