---
name: simple-lang
description: >-
  Plain EN/RU prose: simplify, rewrite, review, translate. Based on ASD-STE100
  for English and информационный стиль for Russian. Not for marketing, pitch,
  or creative copy. Triggers: «упрости текст», «перепиши», «убери канцелярит»,
  «примени STE», «убери двусмысленность», «переведи», «напиши инструкцию»,
  «перепиши по-русски», "plain English", "STE", "disambiguate". Use for docs,
  runbooks, instructions, UI text, error messages, commit messages, Jira
  comments, and Slack messages. Enforces imperative instructions, active voice,
  no filler, exact values, warnings before actions, one term per concept, kept
  modality, and EN/RU parity.
---

# simple-lang — plain technical English and Russian

Apply these rules to English and Russian prose:

- Documentation, runbooks, and instructions
- UI text and error messages
- Commit messages, Jira comments, and chat replies
- Prompts and translations

Code style rules govern code. Do not apply these rules to marketing, pitch, or creative copy. If the user asks, apply them to this copy too.

Only this file states rules and limits. `references/` holds examples, word lists, full passages, rationale, the translation procedure with the parity contract (`translation.md`), and a derived copy of this file (`injection-block.md`). No reference changes a rule.

Shared directives D1–D12 mean the same rule in both languages. E1–E3 apply to English text only. R1–R5 apply to Russian text only. Cite every violation by its ID. If a phrase matches a D-rule and an E- or R-rule, cite the D-rule.

## Routing

| Task | Read before you start |
|---|---|
| Write a short text (chat reply, Jira comment, Slack message) | Nothing more. This file is enough. |
| Write a long or publishable document (runbook, Confluence page, README) | `references/english.md` and/or `references/russian.md`, one for each language in the document |
| Review or rewrite English text | `references/english.md` |
| Review or rewrite Russian text | `references/russian.md` |
| Translate EN ↔ RU | `references/translation.md` and the reference of the target language |
| Check a document that has EN and RU versions of the same content | `references/translation.md`, `references/english.md`, and `references/russian.md` |
| Add the always-on protocol to CLAUDE.md or AGENTS.md | `references/injection-block.md` |

Before a review, a rewrite, or a translation, make sure that each reference that the table names for your task is in your context. If a reference is missing or incomplete, read it again. For a full rewrite of a long text, `references/passages.md` shows complete before/after passages.

## Priority of rules

1. **Accuracy.** Never delete necessary technical information to make text shorter. If a rule conflicts with accuracy, restructure the sentence to keep the fact. If the source is ambiguous or incomplete, list the gap as an open question. Do not invent a value, a condition, or a step.
2. **Clarity.** One sentence, one idea. One word, one meaning.
3. **Brevity.** Delete words that carry no information. Stop when the text is unambiguous, not when it is shortest.

Precedence: accuracy (with D12) > hard limits > D-rules > E- and R-rules. E- and R-rules add language detail to the D-rules. They never relax them. An exception in a directive also applies to the hard limit of that directive. If a hard limit conflicts with another rule, split the sentence to keep the content.

## Hard limits

| Parameter | English | Russian | ID |
|---|---|---|---|
| Instruction sentence | ≤ 20 words | ≤ 15 words | D4 |
| Descriptive sentence | ≤ 25 words | ≤ 20 words | D4 |
| Ideas per sentence | 1 | 1 | D4 |
| Actions per instruction sentence | 1, or 2 that you cannot separate | 1, or 2 that you cannot separate | D4 |
| Paragraph | ≤ 6 sentences | ≤ 6 sentences | D4 |
| Conditions per sentence | 1 | 1 | D5 |
| List nesting | ≤ 2 levels | ≤ 2 levels | D10 |
| Nouns in a row (noun cluster) | ≤ 3 | — | E1 |
| Genitive nouns in a row after the head word | — | ≤ 2 | R2 |

Counting: a number with its unit, a code span, a path, a URL, and a hyphenated word each count as one word. A product name counts as one noun (E1). If the first noun of a compound term alone means something else («база» in «база данных»), the term counts as one noun (R2). Otherwise, count each noun: «пул соединений» and «журнал ошибок» are two nouns.

A limit violation is a defect, like a failing test.

## Enforcement

- **Write:** apply the rules while you write, not after.
- **Review:** quote each violating sentence. Name the ID. Give a BAD → GOOD pair.
- **Rewrite:** keep all facts and the modality of the source. Do not add a fact, a value, a condition, or a step that the source does not have. List each gap as an open question.
- **Verdict:**
  1. If a parity item fails in a document with EN and RU versions: BLOCK.
  2. If any other checklist item fails: REWRITE.
  3. If all items pass: APPROVE.

The examples in this skill assume that the writer knows the facts. A rewrite of another person's text does not add them.

External style guides (client brand, legal boilerplate) override this skill only when the document header says so.

---

## Shared directives D1–D12

### D1. Imperative for instructions

Write each instruction in the imperative. The command verb starts the main clause. Only these can come before it:

- A condition (D5), or a clause with "before" or "until" / «перед тем как», «пока не».
- A time, a place, or a scope: "In the Settings window," / «В окне настроек».
- A purpose clause: "To …" / «Чтобы …».
- A sequence word: "Then" / «Затем».
- A label with a colon: "Optional:", "EN:", "RU:", or a D9 label.
- A negation or a frequency word: "Do not", "Never", "Always" / «Не», «Никогда не», «Всегда».

Do not use words of obligation or advice in instructions:

- EN: "should", "must", "need to", "might want to", "it is necessary", "it is recommended".
- RU: «следует», «необходимо», «нужно», «должен», «рекомендуется».

- EN: "It is recommended that users restart the service." → "Restart the service."
- RU: «Пользователю следует ввести токен.» → «Введите токен.»

Mark an optional step explicitly: "Optional: enable the cache." / «Необязательно: включите кэш.»

This rule covers instructions only. A hedge about an uncertain fact stays (D12).

### D2. Active voice

Name the actor of each action.

- EN: "The configuration is loaded by the server." → "The server loads the configuration."
- RU: «Данные удаляются скриптом.» → «Скрипт удаляет данные.»

Exception for a description. Use passive only when both conditions are true:

- The actor is unknown or unimportant.
- The passive sentence is shorter than the same sentence in active voice.

Never use passive in an instruction. A participle that names a resulting state is not passive: "Make sure that the valve is closed." / «Убедитесь, что клапан закрыт.»

RU: passive is a verb in «-ся» with passive meaning («файл удаляется») or a short passive participle that names an event («файл удалён скриптом», «настройки будут применены»). In Russian text, use the exception at most once per paragraph.

### D3. Verbs, not nominalizations

Do not hide an action in a noun that needs an empty verb. Marker: an empty verb + a noun that names the action.

- EN markers: "perform", "carry out", "conduct", "make", "provide" + an action noun. "Perform a check of the configuration." → "Check the configuration."
- RU markers: «произвести», «осуществить», «выполнить», «провести», «обеспечить», «принять» (in either aspect) + a noun that names an action («проверка», «анализ», «решение»). «Произведите проверку конфигурации.» → «Проверьте конфигурацию.»

A verb with an object that is not an action is not a marker: "Run `make build`." / «Выполните `make build`.»

### D4. One idea per sentence, one action per instruction

Split compound instructions into sentences or a list (D10). Do not join sentences with a semicolon. RU: do not end list items with «;».

- EN: "Open the terminal, go to the project root, and run the build." → "1. Open the terminal. 2. Go to the project root. 3. Run the build."
- RU: «Откройте настройки и включите двухфакторную аутентификацию.» → «Откройте настройки. Затем включите двухфакторную аутентификацию.»

Exception: two actions that you cannot separate physically: "Press and hold the button for 3 s." / «Нажмите и удерживайте кнопку 3 с.»

RU: a деепричастный оборот hides a second action (R4).

### D5. Explicit conditions first

Put the condition before the action. Start it with "If", "When", or "After" / «Если», «Когда», «После того как». One condition per sentence. A clause with "before" or "until" / «перед тем как», «пока не» is not a condition. It can follow the action: "Save the file before you exit."

- EN: "Restart the server if the config changed." → "If the config changed, restart the server."
- RU: «Перезапустите сервис в случае обнаружения ошибки.» → «Если есть ошибка, перезапустите сервис.»

Do not use vague conditions:

- EN: "as needed", "where applicable", "if necessary", "in most cases".
- RU: «при необходимости», «по возможности», «где применимо», «в большинстве случаев».

Replace a vague condition with the exact condition. If the source does not give it, do not invent one. In an instruction, mark the step "Optional:" (D1). List the missing condition as an open question.

RU: replace a condition hidden in a prepositional phrase («при наличии», «в случае», «при отсутствии», «при условии») with an «если» clause: «при наличии ошибок» → «если есть ошибки».

### D6. No filler, answer first

Delete every phrase that carries no information. Test: delete the phrase. If the meaning does not change, it was filler.

- Put the answer or the action in the first sentence. Only a warning that protects that action (D9) can come before it. Put the explanation after it.
- Do not start a sentence with an existential opener: "There is", "There are" / «Существует», «Имеется». Name the actor and the action.
- Write "to", not "in order to". Write «чтобы», not «для того чтобы».
- EN: "Please note that you basically just need to run the command." → "Run the command."
- RU: «Следует отметить, что в настоящее время сервис работает.» → «Сейчас сервис работает.»

Scope: in your own replies, do not use greetings, sign-offs, or apologies. Do not describe yourself or your process: "Let me check", "As an AI" / «Давайте я проверю», «Как языковая модель». When you report your own action, name yourself as the actor: "I deleted the cache." / «Я удалил кэш.» A text in the user's voice (a Slack message, an email) keeps its greeting and its actor.

### D7. One term for one concept

Choose one term for each concept. Use this term in the whole document. Do not use synonyms for variety. Keep the official names of APIs, commands, and products exactly.

- EN: "start", not "initiate", "launch", or "kick off".
- RU: «запустить», не «стартовать» или «инициировать».

Define each abbreviation at first use. Exception: an abbreviation that the reader uses every day (API, URL, HTTP, ID). If you are not sure, define it.

RU: repetition of a term is correct. The school rule «не повторяйтесь» does not apply to technical text.

RU: write a Russian word, an established loanword in Cyrillic («сервер», «токен», «скрипт»), or the English term in Latin letters. Do not make your own transliteration or a hybrid of an English stem and Russian affixes: «ремедиация» → «устранение», «задеплоить» → «развернуть» or `deploy`, «воркер» → «обработчик». Use one form per document.

### D8. Facts, values, and formats

Replace an evaluative adjective with a measurement, or delete it. If no metric exists, state the fact without the adjective.

- EN: "very fast" → "p95 = 40 ms".
- RU: «большой файл» → «файл больше 2 ГБ».

Numbers in quantities:

- EN: decimal point. Comma as the thousands separator from 1,000. Space before a unit symbol, no space before %: `2.5 GB`, `1,000 requests`, `200 ms`, `35%`.
- RU: десятичная запятая. Делите числа от 10 000 пробелом на группы по три цифры. Не делите четырёхзначные числа. Ставьте пробел перед единицей измерения и знаком %: «2,5 ГБ», «10 000 запросов», «1000», «200 мс», «35 %». В вёрстке пробел неразрывный.
- Identifiers have no separators: ports, years, versions, IDs, and error codes (`port 50051`, `2026`, `v2.10.1`).
- Code, commands, configs, and logs: copy each value exactly from the source.

Dates: write the month as a word, or use ISO 8601: "9 October 2026" / «9 октября 2026 г.» / `2026-10-09`. Do not write `10/09/2026`.

Quotes: EN "…". RU «…», nested „…“.

Reference code as `path/file.ext:line`. Reference a section by its exact heading.

### D9. Warnings before actions

Put each warning immediately before the step that it protects. If the risk applies to all steps, put the warning before step 1.

| Level | English label | Russian label | Use for |
|---|---|---|---|
| 1 | **WARNING:** | **ОСТОРОЖНО:** | Risk of injury, a security breach, or an irreversible loss of data, money, or access |
| 2 | **CAUTION:** | **ВНИМАНИЕ:** | Risk of failure, downtime, degradation, a recoverable loss, or extra work |
| 3 | **NOTE:** | **ПРИМЕЧАНИЕ:** | Information without risk |

Write the label in bold capitals with a colon. EN: start the text after the label with a capital letter. RU: start it with a lowercase letter. In a warning, state the risk first. If the source gives an action that prevents the risk, write this action after the risk. Never put a risk or an instruction in a note.

- EN: "Run `db:reset`. Note: this deletes all data." → "**WARNING:** `db:reset` deletes all data. Run `db:reset`."
- RU: «Выполните `DROP TABLE sessions;`. Внимание: данные удалятся.» → «**ОСТОРОЖНО:** команда необратимо удаляет таблицу `sessions` со всеми данными. Выполните `DROP TABLE sessions;`.»

### D10. Lists and tables

| Content | Format |
|---|---|
| 2 ordered steps | Two sentences. Start the second with "Then" / «Затем». |
| 3 or more steps | Numbered list |
| 3 or more items with details (a parameter with a type, a condition with an action) | Bulleted list |
| Items with 2 or more attributes each | Table |
| Up to 3 short plain items (1–3 words each) | Inline: "JSON, YAML, and TOML" / «JSON, YAML и TOML» |
| 4 or more short plain items | Bulleted list |

Use a numbered list only when order matters. Do not nest lists deeper than the limit. Use a table or a separate section instead. Quoted examples inside a rule or a word list can stay inline.

### D11. Error messages and UI text

Formula: what happened → why → what to do. Join the first two parts with a colon. Write what to do as an instruction (D1).

- EN: "An error occurred." → "Could not save the file: the disk is full. Delete the files that you do not need. Then try again."
- RU: «Произошла ошибка.» → «Не удалось сохранить файл: диск заполнен. Удалите ненужные файлы. Затем попробуйте снова.»

Buttons and menu items name the action:

- EN: an imperative verb: "Save", "Retry".
- RU: инфинитив совершенного вида: «Сохранить», «Повторить». Переключатель настройки — инфинитив несовершенного вида: «Показывать уведомления». Прописная буква — только в первом слове: «Сохранить изменения». Сохраняйте устоявшиеся подписи: «Отмена», «ОК».

### D12. Keep modality

Keep the certainty of the source. A hedge about an uncertain fact is content. Do not turn a hedge into a fact. Do not add a hedge that the source does not have.

- EN: "The deploy may have failed because of the migration." Keep it. Do not write "The deploy failed because of the migration."
- RU: «Развёртывание, возможно, завершилось с ошибкой из-за миграции». Сохраните оговорку. Не пишите «Развёртывание завершилось с ошибкой из-за миграции».

---

## English only (E1–E3)

- **E1. Noun clusters.** Do not put more nouns in a row than the limit. Break a longer cluster with a preposition or a relative clause. Keep each noun that carries information. Example: "database connection pool size parameter" → "the parameter that sets the size of the database connection pool".
- **E2. Verb forms.**
  - Use simple present, simple past, and simple future. Do not use continuous forms. Use present perfect only for a state that continues now: "The service has not responded since 10:00." Keep a hedge in its source form: "may have failed" (D12).
  - Use an -ing form only as a noun or an adjective: "logging", "incoming request". Do not use it as a verb: "after starting the migration" → "after the migration starts".
  - Use a single-word verb, not a phrasal verb: "start", not "spin up". Exception: an official UI term or a technical verb without a single-word equivalent: "Log in", "Sign up", "back up", "roll back".
- **E3. Articles and word classes.** Keep the articles ("the", "a"). Do not use contractions: write "do not", not "don't". Exception: UI copy and chat messages. Do not use one word as a verb and as a noun in one sentence.

## Только русский (R1–R5)

- **R1. Канцелярит.** Замените штамп одним словом или коротким оборотом. Штампы: «данный», «является», «осуществлять», «в рамках», «с целью», «в целях», «на предмет», «имеет место», «в связи с тем, что», «вышеуказанный», «путём» + существительное. Пример: «в целях проверки» → «чтобы проверить».
- **R2. Цепочки родительного падежа.** Не ставьте после главного слова подряд больше существительных в родительном падеже, чем разрешает лимит. Главное слово — существительное, от которого зависит первое слово цепочки. Падеж главного слова не важен: в «для проверки конфигурации сервера» их два, а не три. Предлог или глагол разрывает цепочку. Пример: «журнал ошибок обработчика очереди» → «журнал ошибок в обработчике очереди».
- **R3. Прямой порядок слов.** Подлежащее → сказуемое → дополнение: «Скрипт создаёт файл», а не «Файл создаёт скрипт». Условие (D5), время или место в начале фразы — не нарушение. Бытийная фраза с подлежащим в конце — тоже норма: «В логах есть ошибки».
- **R4. Деепричастные и причастные обороты.** Не используйте деепричастные обороты в инструкциях: «Открыв файл, проверьте строку 12» → «Откройте файл. Затем проверьте строку 12». В описании деепричастие и сказуемое называют действия одного подлежащего: не пишите «Запустив скрипт, создаётся файл». Замените причастный оборот из трёх и более слов придаточным с «который». Ставьте «который» сразу после слова, к которому он относится. Одиночное причастие и оборот из двух слов допустимы: «настроенный DNS», «созданный скриптом файл».
- **R5. Вид глагола и форма команды.**
  1. Разовая команда — совершенный вид: «Откройте файл».
  2. Повторяющееся действие и запрет — несовершенный вид: «Проверяйте логи каждый день», «Не удаляйте файл».
  3. Предостережение от случайной ошибки — «не» + совершенный вид: «Не перепутайте порты».
  4. Команда — повелительное наклонение на «вы»: «Откройте файл». Не пишите команду инфинитивом («Открыть файл»).
  5. Сохраняйте обращение автора в тексте от имени пользователя: «ты» или «вы». Не смешивайте их в одном тексте.
  6. Кнопки и пункты меню — по D11.

---

## Checklist

Cite each failed item by its ID.

- [ ] Hard limits (D4, D5, D10, E1, R2)
- [ ] D1 imperative, optional steps marked
- [ ] D2 actor named
- [ ] D3 no empty verb + action noun
- [ ] D4 one idea, one action, no semicolons
- [ ] D5 condition first, exact, not invented
- [ ] D6 no filler, answer first
- [ ] D7 one term, abbreviations defined, no RU hybrids
- [ ] D8 exact values, formats of the language
- [ ] D9 warning before its step, correct level
- [ ] D10 list and table thresholds
- [ ] D11 what happened, why, what to do. Button form
- [ ] D12 modality unchanged
- [ ] E1–E3 for English text, R1–R5 for Russian text
- [ ] Priority of rules, item 1: no fact deleted or invented
- [ ] Parity contract for a document with EN and RU versions
