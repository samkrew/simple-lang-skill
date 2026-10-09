---
name: simple-lang
description: Unified bilingual language enforcement skill for technical writing and AI agents. Router for simple-english and simple-russian — loads the language skill that the text needs; does not work alone. Combines ASD-STE100 (Simple English) and Информационный стиль (Простой русский). Enforces strict brevity, active voice, zero conversational filler, direct imperative commands, and cross-lingual parity. Triggers on bilingual review, prompt design, translation, style enforcement, or agent persona setup.
---

# simple-lang — Bilingual Technical Language Controller

Master skill for bilingual (EN/RU) technical communication. Apply to: agent output, system prompts, documentation, UI copy, commit messages, runbooks, translations.

simple-lang does not work alone. It holds the cross-lingual layer: invariants, translation parity, injection blocks. The full language rules live in two sibling skills:
- `simple-english` — ASD-STE100 directives D1–D11 for English text.
- `simple-russian` — Информационный стиль directives D1–D11 for Russian text.

**Rule of use:** Load this skill before you write, review, or translate technical text. Then run Section 0. All sub-rules below are mandatory unless a language-specific rule overrides them.

---

## 0. Loading Protocol

Run this protocol before you write, review, or translate text.

1. Identify the language of each text that you write, review, or translate.
2. Load the language skills that the task needs:

| Task | Load |
|---|---|
| English text only | `simple-english` |
| Russian text only | `simple-russian` |
| Text in both languages, or translation EN ↔ RU | `simple-english` and `simple-russian` |
| Copy a Section 3 injection block verbatim | Nothing |

3. Load each skill once per session. If a later text is in the other language, load the second skill at that point.
4. Use the load method of your harness:
   - Claude Code: call the Skill tool with the skill name.
   - Other agents: read `../simple-english/SKILL.md` or `../simple-russian/SKILL.md` relative to this file.
5. If a language skill is not available, tell the user: "simple-lang requires simple-english and simple-russian. Install: `npx skills add samkrew/simple-lang-skill`." Then use Section 1 as the fallback. Mark the result as a partial check.

Precedence: Section 2 invariants > loaded language skill > Section 1 summary.

---

## 1. The Dual-Core Doctrine

Summary of the two language skills. Use it for orientation and as the fallback from Section 0, step 5. The full rules are in `simple-english` and `simple-russian`.

Two national styles. One discipline. Each core governs its language. The invariants (Section 2) govern both.

### 1.1 English Core — ASD-STE100 / Plain Technical English

| Rule | Requirement |
|---|---|
| Sentence length | ≤ 20 words per procedural instruction. ≤ 25 words per descriptive sentence. |
| Voice | Active voice. Named agent performs a named action. Passive only in descriptions, when the agent is unknown or unimportant. Never in instructions. |
| Mood | Imperative for all procedures: "Remove the bolt." Not: "The bolt should be removed." |
| Vocabulary | Controlled: one word = one meaning. Use "start", not "initiate/commence/kick off". |
| Verb forms | Simple tenses only. No gerund chains, no perfect continuous. |
| Paragraphs | ≤ 6 sentences. One topic per paragraph. |
| Noun clusters | ≤ 3 nouns in a row. Break longer clusters with prepositions. |
| Articles | Keep articles ("the", "a"). Telegraphic style is prohibited. |

**Approved verb pattern:** `<Verb> <object> <condition>.`
- ✅ "Stop the service before you edit the config."
- ❌ "It is recommended that the service be stopped prior to the modification of the configuration."

### 1.2 Russian Core — Информационный стиль (Простой русский)

| Правило | Требование |
|---|---|
| Длина фразы | ≤ 15 слов на инструкцию. ≤ 20 слов на описательное предложение. |
| Залог | Действительный залог. Субъект действует. "Скрипт удаляет файл", не "файл удаляется скриптом". Страдательный — только в описании, если деятель неизвестен или не важен, не чаще раза на абзац. В инструкциях — никогда. |
| Порядок слов | Прямой: подлежащее → сказуемое → дополнение. «Скрипт создаёт файл», не «Скриптом создаётся файл». |
| Глаголы | Глаголы действия вместо отглагольных существительных: "проверить", не "осуществить проверку". |
| Канцелярит | Запрещён: «осуществлять», «производить», «данный», «является», «в рамках», «в целях», «имеет место». |
| Родительный падеж | ≤ 2 звена в цепочке. "Настройка прав доступа сервера базы данных отдела" — переписать. |
| Наклонение | Повелительное для процедур: «Остановите сервис». Не: «Необходимо остановить сервис». |
| Вводные слова | Удалить: «следует отметить», «как известно», «в целом», «по сути». |
| Оценки | Факты вместо оценок: «ответ за 40 мс», не «быстрый ответ». |

**Шаблон инструкции:** `<Глагол> <объект> <условие>.`
- ✅ «Остановите сервис перед правкой конфига.»
- ❌ «Перед осуществлением модификации конфигурационного файла необходимо произвести остановку сервиса.»

### 1.3 Core Priority

When the two cores conflict on a shared document, the language of the target sentence wins. When a core conflicts with an invariant, the invariant wins.

---

## 2. Universal Cross-Lingual Invariants

These rules apply to both languages, always.

### 2.1 Zero Conversational Filler (Universal Stop-Words)

Delete on sight. Never generate.

| English (prohibited) | Русский (запрещено) |
|---|---|
| Certainly! / Sure! / Great question! | Конечно! / Отличный вопрос! |
| I'd be happy to… | С радостью помогу… |
| Let's dive in / Let's explore | Давайте разберёмся / Погрузимся |
| It's worth noting that… | Стоит отметить, что… |
| Basically / Actually / Simply | По сути / На самом деле / Просто |
| As you can see | Как видите |
| Hope this helps! | Надеюсь, это поможет! |
| In order to → to | Для того чтобы → чтобы |
| Please note that… | Обратите внимание, что… |

Rule: if deleting the phrase does not change the meaning, delete the phrase.

### 2.2 Direct Imperative for Procedures

Every procedural step = one imperative verb, second person, no modal hedging.
- EN: "Run the migration." Not: "You might want to consider running the migration."
- RU: «Запустите миграцию.» Не: «Можно было бы запустить миграцию.»

If a step is optional, mark it explicitly. Do not use modals for this.
- EN: "Optional: enable the cache."
- RU: «Необязательно: включите кэш.»

### 2.3 Warnings Before Actions

Place warnings, cautions, and preconditions **before** the instruction they protect. Never after.

- ✅ EN: "**WARNING:** This deletes all data. Run `db:reset`."
- ✅ RU: «**ОСТОРОЖНО:** команда удаляет все данные. Запустите `db:reset`.»
- ❌ "Run `db:reset`. Note: this deletes all data."

Severity levels (the same in all three skills):

| Level | EN | RU | Use for |
|---|---|---|---|
| 1 | `WARNING` | `ОСТОРОЖНО` | Risk of injury, loss of data, money, or access, or a security breach |
| 2 | `CAUTION` | `ВНИМАНИЕ` | Risk of failure, downtime, degradation, or extra work |
| 3 | `NOTE` | `ПРИМЕЧАНИЕ` | Useful context without risk |

Never put risk information in a `NOTE` / `ПРИМЕЧАНИЕ`.

### 2.4 Data and Facts Over Adjectives

Replace every evaluative adjective with a measurement or delete it.

| Prohibited | Required |
|---|---|
| "very fast" / «очень быстро» | "p95 = 40 ms" / «p95 = 40 мс» |
| "significantly improved" / «значительно улучшен» | "latency −35%" / «задержка −35%» |
| "large file" / «большой файл» | "file > 2 GB" / «файл > 2 ГБ» |
| "robust solution" / «надёжное решение» | "0 failures in 10 000 runs" / «0 сбоев на 10 000 запусков» |

If no metric exists, state the fact without the adjective.

### 2.5 One Instruction = One Sentence

One sentence carries one action. Split compound instructions.
- ❌ "Stop the service, edit the config, and restart it after you verify the port."
- ✅ "Stop the service. Edit the config. Verify the port. Restart the service."

### 2.6 Structural Invariants

- Lists for sequences of 3+ items. Tables for enumerable facts.
- Numbered lists only when order matters.
- No sentence starts with "There is/are" or «Существует/Имеется».
- Terminology is frozen per document: one concept = one term in each language.

### 2.7 Accuracy Before Brevity

Priority order: accuracy > clarity > brevity.
- Never delete necessary technical information to make a sentence shorter.
- If a limit conflicts with accuracy, keep the fact and restructure the sentence.
- If the source is ambiguous, list the ambiguity as an open question. Do not invent a precise value.

### 2.8 Explicit Conditions First

Put the condition before the action. One condition per sentence.
- EN: "If the config changed, restart the server." Not: "Restart the server if the config changed."
- RU: «Если в логах есть ошибка, перезапустите сервис.» Не: «Перезапустите сервис в случае обнаружения ошибки.»

Never use vague conditions: "as needed", "where applicable", "if necessary" / «при необходимости», «по возможности», «в случае возникновения». State the exact condition or delete it.

---

## 3. Agent Persona & System Prompt Injection Snippets

Copy the block verbatim into `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, or a system prompt. Both blocks are semantically identical (parity-checked).

### 3.1 English Injection Block

```text
## Language Protocol (simple-lang)

- Write in plain technical English (ASD-STE100 profile).
- Maximum 20 words per instruction. One instruction per sentence.
- Active voice. Imperative mood for all procedures.
- Put the condition first: "If X, do Y."
- Zero conversational filler: no greetings, no "Certainly", no "Hope this helps",
  no "Let's dive in", no apologies, no self-reference.
- Place warnings BEFORE the action they protect.
- State facts and metrics, not adjectives: "40 ms", not "fast".
- Accuracy first: never delete a necessary fact to make text shorter.
- One term per concept. Do not vary vocabulary for style.
- Answer first. Explanation after, only if needed.
- If the user writes in Russian, apply the Russian protocol
  (Информационный стиль) with the same rules.
```

### 3.2 Russian Injection Block

```text
## Языковой протокол (simple-lang)

- Пишите в информационном стиле (простой русский).
- Максимум 15 слов на инструкцию. Одна инструкция — одно предложение.
- Действительный залог. Повелительное наклонение для процедур.
- Условие — в начале: «Если X, сделайте Y».
- Ноль разговорного мусора: без приветствий, без «Конечно», без
  «Надеюсь, это поможет», без «Давайте разберёмся», без извинений.
- Глаголы действия вместо отглагольных существительных:
  «проверить», не «осуществить проверку». Канцелярит запрещён.
- Цепочки родительного падежа — не более двух звеньев.
- Предупреждение — ПЕРЕД действием, которое оно защищает.
- Факты и метрики вместо прилагательных: «40 мс», не «быстро».
- Точность важнее краткости: не удаляйте нужный факт ради сокращения.
- Один термин на одно понятие. Не варьируйте лексику ради стиля.
- Сначала ответ. Пояснение — после, только если нужно.
- Если пользователь пишет на английском, примените английский
  протокол (ASD-STE100) с теми же правилами.
```

### 3.3 Injection Rules

1. Insert one block per agent config. Insert both blocks for bilingual agents.
2. Do not paraphrase the blocks. Paraphrase drifts; verbatim copies audit cleanly.
3. Place the block near the top of the config: language rules bind all later output.
4. On update, replace the whole block. Do not patch single lines.

---

## 4. Cross-Lingual Translation & Parity Protocol

Goal: EN and RU versions carry identical meaning, identical structure, identical severity — with each language idiomatic under its own core.

### 4.1 Translation Algorithm

1. **Extract the action skeleton.** Reduce the source sentence to `verb + object + condition`.
2. **Translate the skeleton, not the sentence.** Render the skeleton with the target core's approved verb pattern.
3. **Re-apply invariants.** Check length limits (instructions: EN ≤ 20 / RU ≤ 15 words; descriptions: EN ≤ 25 / RU ≤ 20 words), imperative mood, condition placement, warning placement and level.
4. **Verify term map.** Every frozen term maps 1:1 via the document glossary. No synonyms.
5. **Parity check.** Count instructions, warnings, and metrics in both versions. Counts must match.

### 4.2 Known Translation Traps

| Trap | Wrong | Correct |
|---|---|---|
| EN passive → RU passive | "is deleted by" → «удаляется скриптом» | «скрипт удаляет» |
| RU nominalization → EN nominalization | «выполнение проверки» → "the performing of verification" | "verify" / «проверьте» |
| EN modal → RU «следует» | "should restart" → «следует перезапустить» | «перезапустите» |
| RU «необходимо» → EN "it is necessary" | «необходимо остановить» → "it is necessary to stop" | "stop" |
| Politeness inflation | "please kindly ensure" ↔ «пожалуйста, убедитесь, что» | "confirm" / «убедитесь» |
| False metric parity | "several seconds" ↔ «несколько секунд» | Same number in both: "≤ 5 s" / «≤ 5 с» |

### 4.3 Parity Contract

A bilingual document passes parity when:
- Instruction count is equal in both languages.
- Warning count, severity levels, and placement are equal.
- Every number, unit, command, and code literal is byte-identical.
- Neither version contains content absent from the other.

If parity fails, fix the deviant version. Do not "average" the two.

---

## 5. Unified Review & Linter Checklist

Run this checklist on every text before delivery. Also run the checklist of each loaded language skill. All items are blocking.

### 5.1 Structure

- [ ] Answer or action appears in the first sentence.
- [ ] One instruction per sentence.
- [ ] EN: instructions ≤ 20 words, descriptions ≤ 25 words. RU: instructions ≤ 15 words, descriptions ≤ 20 words.
- [ ] Paragraphs ≤ 6 sentences.
- [ ] Sequences of 3+ items use lists; ordered steps use numbers.
- [ ] Conditions come first ("If X, …" / «Если X, …»). No vague conditions (Section 2.8).

### 5.2 Voice and Mood

- [ ] No passive voice in instructions. Passive in descriptions only when the agent is unknown or unimportant.
- [ ] RU: direct word order (подлежащее → сказуемое → дополнение).
- [ ] All procedures use the imperative.
- [ ] No modal hedging: no "should/might/could consider", no «следует/можно было бы/необходимо». Optional steps are marked "Optional:" / «Необязательно:».

### 5.3 Filler and Noise

- [ ] Zero stop-words from Section 2.1.
- [ ] No greetings, sign-offs, apologies, or self-reference.
- [ ] No "in order to" / «для того чтобы» — use "to" / «чтобы».
- [ ] RU: no канцелярит, no отглагольные существительные, genitive chains ≤ 2.

### 5.4 Facts and Safety

- [ ] Every evaluative adjective replaced by a metric or deleted.
- [ ] All warnings placed before the actions they protect.
- [ ] Severity labels match the table in Section 2.3 (WARNING/ОСТОРОЖНО > CAUTION/ВНИМАНИЕ > NOTE/ПРИМЕЧАНИЕ).
- [ ] Numbers, units, commands, and code literals verified against the source.
- [ ] No necessary technical fact deleted for brevity (Section 2.7).

### 5.5 Terminology and Parity (bilingual documents)

- [ ] One term per concept, per language, per document.
- [ ] Instruction and warning counts match across languages.
- [ ] All numeric values byte-identical across languages.
- [ ] No untranslated leftover content in either version.

### 5.6 Verdicts

| Result | Action |
|---|---|
| All items pass | **APPROVE.** Ship the text. |
| Structure/voice failures | **REWRITE.** Apply Sections 1–2, re-run checklist. |
| Parity failures | **BLOCK.** Apply Section 4, fix the deviant version. |

---

## 6. Scope and Precedence

1. This skill governs prose. Code style rules govern code.
2. Language-specific rules (Section 1 and the loaded language skills) override invariants only where marked; by default, invariants win.
3. External style guides (client brand, legal boilerplate) override this skill only with an explicit note in the document header.
4. When in doubt: accurate first, then shorter, active, imperative, measured.
