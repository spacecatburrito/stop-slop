# Changelog

## 2026-05-21

### Major expansion — full humanizer skill

Skill scope expanded from a flat phrase/structure ban-list into a full humanizer with detection, rewrite, voice, scoring, and bilingual support.

**New reference files:**
- `references/ai-tells.md` — 26 AI patterns with stable IDs (TELL-001..026), examples and fixes in EN + RU.
- `references/rewrite-principles.md` — Concrete rewrite moves (cut filler → replace abstractions → break rhythm → apply voice).
- `references/content-modes.md` — Format-specific rules for LinkedIn, email, Telegram DM/channel, proposal, website, social post, academic.
- `references/voice-profiles.md` — Six base voices on four axes (operator, founder, creator, neutral-pro, analyst, casual).
- `references/ru-specific.md` — Russian-specific tells: «является», «позволяет», «осуществлять», calques, participial chains, voice deltas.
- `references/scoring.md` — 0–10 AI-ness rubric across five dimensions.
- `references/examples.md` — Before/after pairs calibrated to TELL IDs, EN + RU.

**New SKILL.md:**
- Inputs: `language`, `mode`, `content_type`, `voice`, `strictness`, `preserve`.
- Three-layer model: detect → rewrite → voice.
- Four output modes: `check`, `rewrite`, `variants`, `score-only`.
- Composition pattern for calling from other agents (writing agents, contract drafts, post-writers).

**New patterns added in this release (not in prior versions):**
- TELL-024 — Triple-negation + em-dash + "just" formula ("No X, no Y, no Z — just W").
- TELL-025 — Standalone "By design." / "On purpose." / "Deliberately." tag.
- TELL-026 — "X is the headline" / "the real story is X" framing-about-framing.

**Removed:**
- `references/phrases.md` and `references/structures.md` — their content was reorganized into `ai-tells.md` (with stable IDs) and `rewrite-principles.md`.

## 2026-01-13

### Added

**Phrases (references/phrases.md)**
- Throat-clearing: "Here's what I find interesting", "Here's the problem though"
- Performative emphasis: "creeps in", "I promise", "They exist, I promise"
- Telling instead of showing: "This is genuinely hard", "This is what leadership actually looks like"

**Structures (references/structures.md)**
- Binary contrasts: "Not X. But Y.", "It's not this. It's that.", "stops being X and starts being Y"
- Rhythm patterns: staccato fragmentation, dashes for dramatic pause, hedging as reassurance
- Word patterns: absolute words (always, never, everyone, etc.), AI-overused intensifiers (deeply, truly, fundamentally, inherently, simply, literally, inevitably)

## 2026-01-12

- Restructured skill following Claude Code best practices (PR #1)
- Split into SKILL.md and references/ folder

## 2025-01-12

- Initial release
