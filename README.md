# simple-lang-skill

> One agent skill that enforces clear, minimal, unambiguous writing — rules based on **ASD-STE100** (Simplified Technical English) for English and **информационный стиль** (Infostyle) for Russian. One doctrine, two languages, zero fluff.

![skills](https://img.shields.io/badge/skills-1-blue) ![agents](https://img.shields.io/badge/agents-Claude%20Code%20%7C%20Hermes%20%7C%20Codex%20%7C%20Cursor-8A2BE2) ![license](https://img.shields.io/badge/license-Unlicense-lightgrey)

Compatible with **Claude Code**, **Hermes Agent**, **Codex**, **Cursor**, and human technical documentation workflows. The skill is portable Markdown — no runtime, no dependencies.

---

## Install

Install for the current project (the default scope). The CLI keeps the copy in `./.agents/skills/` and links it into `./.claude/skills/`:

```bash
npx skills add samkrew/simple-lang-skill -a claude-code -y
```

Install globally for Claude Code:

```bash
npx skills add samkrew/simple-lang-skill -a claude-code -g -y
```

Install globally for Codex:

```bash
npx skills add samkrew/simple-lang-skill -a codex -g -y
```

With `-g`, the CLI keeps the canonical copy in `~/.agents/skills/`. Codex also reads that folder. Keep one Codex copy: if `~/.codex/skills/simple-lang` also exists, delete one of the two.

Manual install for **Hermes Agent** (copy the whole folder, including `references/`):

```bash
git clone https://github.com/samkrew/simple-lang-skill.git /tmp/simple-lang-skill
mkdir -p ~/.hermes/skills
cp -r /tmp/simple-lang-skill/skills/simple-lang ~/.hermes/skills/
rm -rf /tmp/simple-lang-skill
```

---

## Migration from 1.x

Version 2.0.0 replaces the three 1.x skills (`simple-lang` router, `simple-english`, `simple-russian`) with one skill, `simple-lang`.

1. Install 2.0.0 as shown above.
2. Remove the old `simple-english` and `simple-russian` skills. If you installed them with npx, run `npx skills remove -g simple-english simple-russian`. Otherwise delete the two folders from every skills directory you use (for example `~/.claude/skills/`, `~/.agents/skills/`, `~/.codex/skills/`, `~/.hermes/skills/`). An update can leave removed skills in place. An agent can then load an old skill for a single-language request and apply the 1.x rules.
3. If you pasted a 1.x snippet into `CLAUDE.md` or `AGENTS.md`, replace it with the block from [`references/injection-block.md`](skills/simple-lang/references/injection-block.md).

What changed in the rules:

- D1–D11 keep their IDs. New: **D12 — keep modality** (a hedge is content).
- D4 also bans semicolons. D6 adds "answer first" and limits the ban on greetings and apologies to the agent's own replies. D8 adds date, quote, and identifier formats. D11 now covers button labels.
- E2 permits the present perfect for a state that continues now, and technical verbs such as "back up" and "roll back".
- Warning labels use one format: bold capitals with a colon (`**WARNING:**`, `**ОСТОРОЖНО:**`).
- Each rule and each limit is stated once, in `SKILL.md`.

Version 1.3.0 stays available at the tag [`v1.3.0`](https://github.com/samkrew/simple-lang-skill/tree/v1.3.0).

---

## How it works

| File | Loaded | Contents |
|---|---|---|
| `SKILL.md` | When the skill triggers | All rules and limits: priority, hard limits, D1–D12, E1–E3, R1–R5, checklist, routing |
| `references/english.md` | For English reviews, rewrites, and long documents | Examples, word lists, differences from ASD-STE100 |
| `references/russian.md` | For Russian reviews, rewrites, and long documents | Примеры и списки слов |
| `references/passages.md` | For a full rewrite of a long text | Complete before/after passages in both languages |
| `references/translation.md` | For EN ↔ RU translation and parity checks | Procedure, traps, parity contract |
| `references/injection-block.md` | When you configure an agent | The always-on block for `CLAUDE.md` / `AGENTS.md` |

Short texts (a chat reply, a Jira comment, a Slack message) need only `SKILL.md`.

### Directive map

| ID | Rule | Applies to |
|---|---|---|
| D1 | Imperative for instructions | EN + RU |
| D2 | Active voice | EN + RU |
| D3 | Verbs, not nominalizations | EN + RU |
| D4 | One idea per sentence, one action per instruction | EN + RU |
| D5 | Explicit conditions first | EN + RU |
| D6 | No filler, answer first | EN + RU |
| D7 | One term for one concept | EN + RU |
| D8 | Facts, values, and formats | EN + RU |
| D9 | Warnings before actions | EN + RU |
| D10 | Lists and tables | EN + RU |
| D11 | Error messages and UI text | EN + RU |
| D12 | Keep modality | EN + RU |
| E1 | Noun clusters | English only |
| E2 | Verb forms, no phrasal verbs | English only |
| E3 | Articles and word classes | English only |
| R1 | Канцелярит | Russian only |
| R2 | Genitive chains | Russian only |
| R3 | Direct word order | Russian only |
| R4 | Деепричастные и причастные обороты | Russian only |
| R5 | Verb aspect and command form | Russian only |

The limits and the full wording are in [`SKILL.md`](skills/simple-lang/SKILL.md).

---

## Core Doctrine & Philosophy

Both methods attack the same problem: **text that makes the reader work harder than necessary.** STE and Infostyle developed independently. STE comes from aerospace maintenance manuals. Infostyle comes from Russian editorial practice. Its authors, Maxim Ilyakhov and Lyudmila Sarycheva, used ideas from Nora Gal's «Слово живое и мёртвое». Both methods share these principles:

1. **One word — one meaning.** In STE, an approved word has only the meaning and the part of speech that the dictionary gives (`follow` means "come after", never "obey"). In Infostyle, a word that can be removed without loss of meaning must be removed.
2. **Verbs carry the sentence.** Active voice, imperative mood for instructions. Nominalizations (`осуществление проверки`, `implementation of the verification`) are compressed dead verbs — resurrect them (`check`, `проверить`).
3. **Short sentences, one idea each.** Each language has hard limits for sentence and paragraph length.
4. **Facts, not evaluations.** "Fast" is an opinion. "Responds in 40 ms" is a fact. Intensifiers (`very`, `крайне`, `максимально`) add emphasis by subtracting trust.
5. **Warnings before actions.** State the risk before the step that needs it.
6. **Certainty stays as it is.** A hedge is content. "May have failed" never becomes "failed".
7. **The reader owes you nothing.** No filler, no throat-clearing, no "as everyone knows". Respect is measured in the reader's saved seconds.

**What this skill does NOT do:** it does not oversimplify text, delete technical terms, or make the tone robotic. Domain terminology is kept. Structure and precision are added. Only noise is removed. It is not for marketing, pitch, or creative copy.

---

## Cheatsheet

### English (based on ASD-STE100)

| ❌ BAD | ✅ GOOD | Rule |
|---|---|---|
| The removal of the panel should be carried out by maintenance personnel. | Remove the panel. | D1, D2, D3 |
| It is important to ensure that the valve is in the closed position prior to commencement of the test. | Before you start the test, close the valve. | D3, D6, D7 |
| The system may possibly experience intermittent failures under certain conditions. | When the temperature is above 70 °C, the system may fail intermittently. | D5, D12 |
| Follow the safety instructions. | Obey the safety instructions. | D7: `follow` = "come after" only |
| Remove the cover. Note: high voltage. | **WARNING:** The terminals under the cover are at 400 V. Disconnect the power. Then remove the cover. | D9 |
| Utilize the appropriate fastening components. | Use the specified fasteners. | D7, D8 |

### Русский (информационный стиль)

| ❌ ПЛОХО | ✅ ХОРОШО | Правило |
|---|---|---|
| Осуществляется процесс обработки данных. | Сервис обрабатывает данные. | D2, D3, R1 |
| Данный функционал является крайне важным для пользователей. | Функция нужна пользователям: без неё нельзя выгрузить отчёт. | D8, R1 |
| В связи с тем, что произошёл сбой, было принято решение о перезапуске. | Из-за сбоя мы решили перезапустить сервер. | D2, D3, R1 |
| Следует отметить, что настройка производится достаточно просто. | Настройка занимает две минуты: заполните три поля. | D2, D6, D8 |
| Не забудьте произвести сохранение изменений. | Сохраните изменения. | D1, D3 |
| Открыв файл, проверьте строку 12. | Откройте файл. Затем проверьте строку 12. | R4 |

The GOOD versions assume that the writer knows the facts. A rewrite of another person's text does not add facts.

---

## Always-on protocol

A skill loads only when a task triggers it. To apply the style to every reply, paste the block from [`references/injection-block.md`](skills/simple-lang/references/injection-block.md) into `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, or a system prompt.

---

## Skill maintenance

- State each rule and each limit once, in `SKILL.md`. The files in `references/` hold examples, word lists, full passages, rationale, the translation procedure, and the derived always-on block.
- Before a release, check that no limit leaked into the references. Only `injection-block.md` (a derived copy) may match:

  ```bash
  grep -niE '≤ ?[0-9]+ ?(words|слов|nouns|sentences|предложений)|at most|не больше|не чаще' skills/simple-lang/references/*.md
  ```

- Keep each reference at or below 16 KB, so that agents can restore it after a context compaction. This command must print nothing:

  ```bash
  find skills/simple-lang/references -name '*.md' -size +16384c
  ```

- Keep the body of `SKILL.md` (without the frontmatter) below 19,900 characters. Claude Code re-attaches at most about 20,000 characters of a skill after a context compaction:

  ```bash
  awk 'n>=2; /^---$/{n++}' skills/simple-lang/SKILL.md | wc -m
  ```

- After each change to `SKILL.md`, update `references/injection-block.md`.
- Keep `references/*.md` one level deep. Never name a reference file `SKILL.md`: some agents register every `SKILL.md` as a separate skill.

---

## Repository Structure

```
simple-lang-skill/
├── skills/
│   └── simple-lang/
│       ├── SKILL.md                    # all rules and limits (EN + RU)
│       └── references/
│           ├── english.md              # English examples, word lists, STE differences
│           ├── russian.md              # Russian examples and word lists
│           ├── passages.md             # full before/after passages (EN + RU)
│           ├── translation.md          # EN ↔ RU procedure and parity contract
│           └── injection-block.md      # always-on block for CLAUDE.md / AGENTS.md
├── .claude-plugin/
│   └── plugin.json                     # Claude Code plugin manifest
├── README.md
├── LICENSE
└── .gitignore
```

---

## License

This is free and unencumbered software released into the public domain under [The Unlicense](https://unlicense.org).
