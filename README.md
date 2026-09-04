# simple-lang-skill

> Agent skills that enforce clear, minimal, unambiguous writing — **ASD-STE100** (Simplified Technical English) for English and **Информационный стиль** (Infostyle) for Russian. One doctrine, two languages, zero fluff.

![skills](https://img.shields.io/badge/skills-3-blue) ![agents](https://img.shields.io/badge/agents-Claude%20Code%20%7C%20Hermes%20%7C%20Codex%20%7C%20Cursor-8A2BE2) ![license](https://img.shields.io/badge/license-Unlicense-lightgrey)

Compatible with **Claude Code**, **Hermes Agent**, **Codex**, **Cursor**, and human technical documentation workflows. Skills are portable Markdown — no runtime, no dependencies.

---

## Install

Install the full pack:

```bash
npx skills add samkrew/simple-lang-skill
```

Install a single skill:

```bash
npx skills add samkrew/simple-lang-skill --skill simple-english
npx skills add samkrew/simple-lang-skill --skill simple-russian
```

Install project-local (writes to `./.claude/skills/` instead of `~/.claude/skills/`):

```bash
npx skills add samkrew/simple-lang-skill -p
```

CI / non-interactive install (target Claude Code, global, auto-confirm):

```bash
npx skills add samkrew/simple-lang-skill -a claude-code -g -y
```

Manual install for **Hermes Agent**:

```bash
git clone https://github.com/samkrew/simple-lang-skill.git /tmp/simple-lang-skill
cp -r /tmp/simple-lang-skill/skills/* ~/.hermes/skills/
rm -rf /tmp/simple-lang-skill
```

---

## Catalog

| Skill | Language | Purpose |
|---|---|---|
| `simple-lang` | Bilingual | Master controller skill. Enforces universal cross-lingual invariants, ASD-STE100 for English, Infostyle for Russian, and translation parity. |
| `simple-english` | English | Enforces ASD-STE100: one word — one meaning, approved verbs, active voice, short sentences (≤ 20 words for instructions), procedural clarity. |
| `simple-russian` | Russian | Enforces Информационный стиль: no stop-words, no bureaucratese (канцелярит), active voice, facts over evaluations, verbs over verbal nouns. |

---

## Core Doctrine & Philosophy

Both standards attack the same problem: **text that makes the reader work harder than necessary.** They were developed independently — STE for aerospace maintenance manuals, Infostyle (Maxim Ilyakhov, Nora Gal) for Russian editorial practice — and converge on the same principles:

1. **One word — one meaning.** In STE, each approved word has exactly one part of speech and one definition (`follow` means "come after", never "obey"). In Infostyle, a word that can be removed without loss of meaning must be removed.
2. **Verbs carry the sentence.** Active voice, imperative mood for instructions. Nominalizations (`осуществление`, `implementation of the verification`) are compressed dead verbs — resurrect them (`verify`, `проверить`).
3. **Short sentences, one idea each.** STE hard limits: **≤ 20 words** per procedural sentence, **≤ 25 words** per descriptive sentence, **≤ 6 sentences** per paragraph. Infostyle limit: **≤ 15 words** per instruction, genitive chains ≤ 2 words. If a sentence needs a second read, split it.
4. **Facts, not evaluations.** "Fast" is an opinion. "Responds in 40 ms" is a fact. Delete intensifiers (`very`, `крайне`, `максимально`) — they add emphasis by subtracting trust.
5. **Warnings before actions.** State the caution, then the instruction. Never bury a safety condition after the step that needs it.
6. **The reader owes you nothing.** No filler, no throat-clearing, no "as everyone knows". Respect is measured in the reader's saved seconds.

**What these skills do NOT do:** they do not dumb text down, strip technical terms, or flatten tone into robotese. Domain terminology is kept. Structure and precision are added. Only noise is removed.

---

## Cheatsheet

### English (ASD-STE100)

| ❌ BAD | ✅ GOOD | Rule |
|---|---|---|
| The removal of the panel should be carried out by maintenance personnel. | Remove the panel. | Active voice, imperative, no nominalization |
| It is important to ensure that the valve is in the closed position prior to commencement of the test. | Close the valve before you start the test. | Approved words, ≤ 20 words |
| The system may possibly experience intermittent failures under certain conditions. | The system can fail when the temperature is more than 70 °C. | Facts, not hedging |
| Follow the safety instructions. | Obey the safety instructions. | `follow` = "come after" only |
| Turn off the power. Warning: high voltage. | **WARNING: HIGH VOLTAGE.** Turn off the power. | Warning before action |
| Utilize the appropriate fastening components. | Use the correct bolts. | Simple approved verbs, concrete nouns |

### Русский (Информационный стиль)

| ❌ ПЛОХО | ✅ ХОРОШО | Правило |
|---|---|---|
| Осуществляется процесс обработки данных. | Сервис обрабатывает данные. | Глагол вместо отглагольного существительного |
| Данный функционал является крайне важным для пользователей. | Функция нужна пользователям: без неё нельзя выгрузить отчёт. | Факт вместо оценки |
| В связи с тем, что произошёл сбой, было принято решение о перезапуске. | Сервер сбоил, поэтому мы его перезапустили. | Убрать канцелярит, назвать деятеля |
| Следует отметить, что настройка производится достаточно просто. | Настройка занимает две минуты: заполните три поля. | Убрать вводные, дать измеримое |
| Не забудьте произвести сохранение изменений. | Сохраните изменения. | Императив, минус стоп-слова |
| Наша команда высококвалифицированных специалистов оказывает полный спектр услуг. | Мы настраиваем CI/CD за неделю. Список услуг — ниже. | Конкретика вместо самопохвалы |

---

## Agent System Prompt Snippets

Paste into `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, or any agent system prompt.

### English (STE mode)

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

### Русский (Инфостиль)

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

---

## Repository Structure

```
simple-lang-skill/
├── skills/
│   ├── simple-lang/SKILL.md       # bilingual router & meta-controller
│   ├── simple-english/SKILL.md    # ASD-STE100 rules & directives
│   └── simple-russian/SKILL.md    # информационный стиль rules & directives
├── .claude-plugin/
│   └── plugin.json                # Claude Code plugin manifest
├── README.md
├── LICENSE
└── .gitignore
```

---

## License

This is free and unencumbered software released into the public domain under [The Unlicense](https://unlicense.org).
