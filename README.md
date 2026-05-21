# Stop Slop

A skill for detecting AI writing patterns and rewriting prose into sharper, human, context-aware language.

<img width="3840" height="2160" alt="G-Yg4RVbIAAhVxW" src="https://github.com/user-attachments/assets/902afc15-1f40-4a9d-af24-8cd67afb8ebf" />

## What this is

AI writing has patterns. Predictable phrases, structures, rhythms. This skill teaches Claude (or any LLM) to catch and remove them.

Bilingual: English + Russian. Six pluggable voice profiles. Modes for diagnosis, rewrite, three-variant generation, and score-only output.

## Skill structure

```
stop-slop/
├── SKILL.md                              # Core instructions, inputs, hard rules, process
├── references/
│   ├── ai-tells.md                       # 26 AI patterns with IDs, examples, fixes (EN + RU)
│   ├── rewrite-principles.md             # How to rewrite — concrete moves, not platitudes
│   ├── content-modes.md                  # Format-specific rules: LinkedIn, email, Telegram, proposal, website, academic
│   ├── voice-profiles.md                 # Six base voices: operator, founder, creator, neutral-pro, analyst, casual
│   ├── ru-specific.md                    # Russian-specific tells: «является», «позволяет», calques, participial chains
│   ├── scoring.md                        # 0–10 AI-ness scoring on five dimensions
│   └── examples.md                       # Before/after pairs, EN + RU, calibrated to TELL IDs
├── README.md
├── CHANGELOG.md
└── LICENSE
```

## Quick start

**Claude Code:** Drop this folder into `.claude/skills/`. The skill loads on demand.

**Claude Projects:** Upload `SKILL.md` and reference files to project knowledge.

**API calls:** Include `SKILL.md` in the system prompt. Reference files load on demand (`ai-tells.md` always; others by mode and content type).

**Custom instructions:** Copy the hard-rules block from `SKILL.md` into your system prompt.

## Inputs

| Field | Values | Default |
|---|---|---|
| `text` | string | required |
| `language` | `en` \| `ru` \| `auto` | `auto` |
| `mode` | `check` \| `rewrite` \| `variants` \| `score-only` | `rewrite` |
| `content_type` | `linkedin` \| `email` \| `telegram-dm` \| `telegram-channel` \| `proposal` \| `website` \| `social-post` \| `academic` \| `general` | `general` |
| `voice` | `operator` \| `founder` \| `creator` \| `neutral-pro` \| `analyst` \| `casual` | `operator` |
| `strictness` | `light` \| `standard` \| `aggressive` | `standard` |
| `preserve` | list of strings | `[]` |

## What it catches

**26 AI tells** (see `references/ai-tells.md`). A few examples:

- **TELL-001** Em-dash overuse — sentence rhythm built on `—` instead of periods or commas.
- **TELL-002** Fake contrast formulas — "It's not X, it's Y" / "это не про X, это про Y" and split-across-sentence variants.
- **TELL-005** LinkedIn fragment chains — "Clear. Concise. Effective."
- **TELL-019** Setup-payoff colons — "The loop:", "The catch:", «Расклад:».
- **TELL-021** Arrow-step sequences — "watch → learn → replace".
- **TELL-023** Two-sentence-thrust rhythm — when every paragraph is 1–2 sentences of setup→reveal.
- **TELL-024** Triple-negation + em-dash + "just" — "No X, no Y, no Z — just W".
- **TELL-025** Standalone "By design." / "On purpose." / "Deliberately." tags.
- **TELL-026** "X is the headline" / "the real story is X" framings.

Each tell has examples in English and Russian (where relevant), explanation, and fix direction.

## Voices

Six base profiles, each calibrated on four axes (Formal↔Casual, Soft↔Assertive, Abstract↔Concrete, Analytical↔Narrative):

- `operator` (default) — Direct, commercially aware, skeptical of fluff.
- `founder` — First-person, opinionated, narrative-leaning.
- `creator` — Personable, lower-formality, conversational rhythm.
- `neutral-pro` — Professional, restrained, no jargon.
- `analyst` — Argument-first, evidence-led, abstract-leaning.
- `casual` — Plain spoken, mostly short sentences.

Extend with custom voices by injecting 3–5 samples plus a base profile as fallback. See `references/voice-profiles.md`.

## Scoring

Rate 0–10 on each dimension:

| Dimension | Question |
|---|---|
| Directness | Does it state the claim or announce it? |
| Rhythm | Varied or metronomic? |
| Trust | Respects reader intelligence? |
| Authenticity | Sounds human? |
| Density | Anything cuttable? |

Below 6/10 average: revise. Full scoring rubric in `references/scoring.md`.

## Bilingual

EN and RU first-class. RU has its own tells (verb-noun bloat «осуществлять выполнение», participial chains, «является», «позволяет», «данный»). See `references/ru-specific.md`.

Voice profiles work in both languages with deltas noted per profile.

## Composing from agents

```
1. Agent drafts content per its own template.
2. Agent invokes stop-slop with:
     mode=rewrite
     voice=<agent's house voice>
     content_type=<format>
     preserve=<facts/numbers/names from source>
3. Agent returns the rewritten output.
```

`preserve` is required when calling from agents — the rewrite is aggressive on filler but should never touch protected content.

## Credits

Built on top of the original [stop-slop](https://github.com/hardikpandya/stop-slop) by [Hardik Pandya](https://hvpandya.com). The detection rules, voice profiles, scoring rubric, and bilingual support were added on top.

## License

MIT. Use freely, share widely.
