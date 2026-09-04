---
name: simple-lang
description: Unified bilingual language enforcement skill for technical writing and AI agents. Combines ASD-STE100 (Simple English) and Информационный стиль (Простой русский). Enforces strict brevity, active voice, zero conversational filler, direct imperative commands, and cross-lingual parity. Triggers on bilingual review, prompt design, translation, style enforcement, or agent persona setup.
---

# simple-lang — Bilingual Technical Language Controller

Master skill for bilingual (EN/RU) technical communication. Apply to: agent output, system prompts, documentation, UI copy, commit messages, runbooks, translations.

**Rule of use:** Load this skill before you write, review, or translate technical text. All sub-rules below are mandatory unless a language-specific rule overrides them.

---

## 1. The Dual-Core Doctrine

Two national styles. One discipline. Each core governs its language. The invariants (Section 2) govern both.

### 1.1 English Core — ASD-STE100 / Plain Technical English

| Rule | Requirement |
|---|---|
| Sentence length | ≤ 20 words per procedural instruction. ≤ 25 words per descriptive sentence. |
| Voice | Active voice only. Named agent performs a named action. |
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
| Длина фразы | ≤ 15 слов на инструкцию. |
| Залог | Действительный залог. Субъект действует. "Скрипт удаляет файл", не "файл удаляется скриптом". |
| Глаголы | Глаголы действия вместо отглагольных существительных: "проверить", не "осуществить проверку". |
| Канцелярит | Запрещён: «осуществлять», «производить», «данный», «является», «в рамках», «в целях», «имеет место». |
| Родительный падеж | ≤ 2 звена в цепочке. "Настройка прав доступа сервера базы данных отдела" — переписать. |
| Наклонение | Повелительное для процедур: «Остановите сервис». Не: «Необходимо остановить сервис». |
| Вводные слова | Удалить: «следует отметить», «как известно», «в целом», «по сути». |
| Оценки | Факты вместо оценок: «ответ за 40 мс», не «быстый ответ». |

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

### 2.3 Warnings Before Actions

Place warnings, cautions, and preconditions **before** the instruction they protect. Never after.

- ✅ EN: "**WARNING:** This deletes all data. Run `db:reset`."
- ✅ RU: «**ВНИМАНИЕ:** команда удаляет все данные. Запустите `db:reset`.»
- ❌ "Run `db:reset`. Note: this deletes all data."

Severity order: `WARNING/ВНИМАНИЕ` (injury, data loss) > `CAUTION/ОСТОРОЖНО` (damage, degradation) > `NOTE/ПРИМЕЧАНИЕ` (context).

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

---

## 3. Agent Persona & System Prompt Injection Snippets

Copy the block verbatim into `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, or a system prompt. Both blocks are semantically identical (parity-checked).

### 3.1 English Injection Block

```text
## Language Protocol (simple-lang)

- Write in plain technical English (ASD-STE100 profile).
- Maximum 20 words per instruction. One instruction per sentence.
- Active voice only. Imperative mood for all procedures.
- Zero conversational filler: no greetings, no "Certainly", no "Hope this helps",
  no "Let's dive in", no apologies, no self-reference.
- Place warnings BEFORE the action they protect.
- State facts and metrics, not adjectives: "40 ms", not "fast".
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
- Только действительный залог. Повелительное наклонение для процедур.
- Ноль разговорного мусора: без приветствий, без «Конечно», без
  «Надеюсь, это поможет», без «Давайте разберёмся», без извинений.
- Глаголы действия вместо отглагольных существительных:
  «проверить», не «осуществить проверку». Канцелярит запрещён.
- Цепочки родительного падежа — не более двух звеньев.
- Предупреждение — ПЕРЕД действием, которое оно защищает.
- Факты и метрики вместо прилагательных: «40 мс», не «быстро».
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
3. **Re-apply invariants.** Check length limit (EN ≤ 20 / RU ≤ 15 words), imperative mood, warning placement.
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

Run this checklist on every text before delivery. All items are blocking.

### 5.1 Structure

- [ ] Answer or action appears in the first sentence.
- [ ] One instruction per sentence.
- [ ] EN sentences ≤ 20 words (procedures); RU ≤ 15 words.
- [ ] Paragraphs ≤ 6 sentences.
- [ ] Sequences of 3+ items use lists; ordered steps use numbers.

### 5.2 Voice and Mood

- [ ] Zero passive voice (EN). Ноль страдательного залога (RU).
- [ ] All procedures use the imperative.
- [ ] No modal hedging: no "should/might/could consider", no «следует/можно было бы/необходимо».

### 5.3 Filler and Noise

- [ ] Zero stop-words from Section 2.1.
- [ ] No greetings, sign-offs, apologies, or self-reference.
- [ ] No "in order to" / «для того чтобы» — use "to" / «чтобы».
- [ ] RU: no канцелярит, no отглагольные существительные, genitive chains ≤ 2.

### 5.4 Facts and Safety

- [ ] Every evaluative adjective replaced by a metric or deleted.
- [ ] All warnings placed before the actions they protect.
- [ ] Severity labels used correctly (WARNING > CAUTION > NOTE).
- [ ] Numbers, units, commands, and code literals verified against the source.

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
2. Language-specific core (Section 1) overrides invariants only where marked; by default, invariants win.
3. External style guides (client brand, legal boilerplate) override this skill only with an explicit note in the document header.
4. When in doubt: shorter, active, imperative, measured.
