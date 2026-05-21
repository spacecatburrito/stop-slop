# Russian-specific rules

Load when `language=ru`. Layer on top of base rules.

Russian AI writing has its own tells, distinct from English. LLMs trained on translated EN content carry calques, formal participial chains, and verb-noun bloat that don't exist in good native RU.

---

## TELL-CORPRU-001 · Verb-noun bloat

**Pattern:** Verb-of-doing + verbal noun, where one plain verb would work.

| AI bloat | Plain |
|---|---|
| осуществлять разработку | разрабатывать |
| производить замеры | замерять |
| провести анализ | проанализировать |
| оказывать поддержку | поддерживать |
| вести работу над | работать над |
| иметь возможность | мочь |

**Fix:** collapse to the verb.

---

## TELL-CORPRU-002 · «Является» / «представляет собой»

**Pattern:** "X является Y", "X представляет собой Y" — instead of "X — это Y" or just "X is Y" structure.

**Why AI:** Translation calque from English copula. Reads as bureaucratic.

**Fix:**
- Use тире: «Продукт — это сервис гуманизации».
- Or restructure: «Продукт делает X».
- Drop entirely if it's empty: «Это сервис гуманизации».

---

## TELL-CORPRU-003 · «Позволяет» as universal verb

**Pattern:** "X позволяет делать Y" — applied to almost any function.

**Why AI:** Translates EN "X enables Y" / "X allows you to do Y". Overused.

**Fix:** Name what X actually does.
- Bad: «Бот позволяет очистить текст от AI-штампов».
- Better: «Бот вычищает AI-штампы из текста».

---

## TELL-CORPRU-004 · «Данный», «указанный», «вышеупомянутый»

**Pattern:** Bureaucratic demonstratives instead of «этот», «тот».

**Why AI:** Formal-document calque.

**Fix:** «этот», «тот», or drop entirely.
- Bad: «Данный продукт решает указанную проблему».
- Better: «Этот продукт решает эту задачу» or «Продукт решает задачу».

---

## TELL-CORPRU-005 · Participial chains

**Pattern:** Long strings of причастия и деепричастия. «Используя предоставленные данные, выполняющие функцию X, реализуя возможности, обеспечивающие Y…»

**Why AI:** EN-to-RU translation produces this. Native Russian breaks them up.

**Fix:** Split into 2–3 sentences with finite verbs.

---

## TELL-CORPRU-006 · Buzzword fog (RU)

Words to challenge:
- оптимизировать (when no specific metric)
- масштабировать (when no specific scale)
- эффективный, эффективность (vague)
- инновационный
- комплексный
- современный
- передовой
- цифровая трансформация
- экосистема (unless actually networked)
- бесшовный (the worst calque from "seamless")
- уникальный (almost always not)

**Rule:** Same as EN — don't auto-delete. Check for meaning. Replace with concrete action / outcome / number.

---

## TELL-CORPRU-007 · Generic openings (RU)

Cut:
- «В современном мире…»
- «В наши дни…»
- «На сегодняшний день…»
- «В условиях постоянно меняющегося рынка…»
- «В эпоху цифровизации…»
- «Ни для кого не секрет, что…»

**Fix:** Open with the specific observation or fact.

---

## TELL-CORPRU-008 · «Стоит отметить», «Важно понимать»

Same as TELL-006 (importance phrases) in EN. RU equivalents:
- «Стоит отметить, что…»
- «Важно отметить…»
- «Хочется подчеркнуть…»
- «Ключевой момент здесь…»
- «Нельзя не упомянуть…»

**Fix:** Delete the phrase, keep the substance.

---

## TELL-CORPRU-009 · Excessive hedging (RU)

Stacked qualifiers:
- «возможно, скорее всего, можно сказать, что…»
- «по всей видимости, в определённой степени…»
- «в целом, в общем-то, по большому счёту…»

**Fix:** One qualifier max. Or commit to the claim.

---

## TELL-CORPRU-010 · Forced parallelism in lists

LLMs over-balance RU lists. Each bullet has the same grammatical structure, same length, same pace.

**Fix:** Vary at least one. Mix infinitive verbs with noun phrases. Let one item be longer or shorter.

---

## RU voice profile deltas

### `operator` (RU)
- Direct, but Russian neutrally allows slightly longer sentences than EN. Don't over-chop.
- Drop «является», «осуществляет», «позволяет» aggressively.
- First-person singular «я» is OK and often correct, despite the cultural pull toward «мы».
- Avoid «коллеги», «команда» as filler subjects.

### `founder` (RU)
- «Я» is the right pronoun.
- Skepticism and irony work well in RU founder voice.
- Avoid «дорогие подписчики», «дорогие друзья» openings.

### `creator` (RU)
- Conversational register: «короче», «ну», «вот», «такая штука» — sparingly, only if the source register supports it.
- First-person «я» dominant.
- Allow incomplete thoughts, ironic asides.

### `neutral-pro` (RU)
- Formal but not bureaucratic. Avoid «уважаемый», «настоящим», «вышеуказанный» unless the document type requires it (contracts).
- «Вы» / «вам» — capitalize only in personal direct address (письмо одному адресату); lowercase in mass communication.

### `casual` (RU)
- Lower-case is fine.
- Drop greetings to one word or none.
- Skip closings.
- One ask per message.

---

## Punctuation notes (RU)

- Тире vs. дефис: AI often confuses them. Use тире (—) for sentence-level pauses, дефис (-) for compounds (онлайн-сервис).
- Кавычки: prefer «ёлочки» as primary, „лапки" as nested. Don't use straight ASCII quotes in published RU prose.
- Емтире — same anti-overuse rule as EN. Don't replace EN em dashes with RU em dashes; restructure.
- Multiple exclamation/question marks: cut to one.

---

## RU rhythm

Russian sentences average 12–18 words in good native prose. AI-generated RU tends toward 20–30 words with subordinate clauses. Break them up.

Target distribution: ~30% short (5–10 words), ~50% medium (11–18), ~20% longer (19–25). Almost never 25+ words in social/web copy.
