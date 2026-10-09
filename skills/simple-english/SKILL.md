---
name: simple-english
description: Enforce ASD-STE100 (Simplified Technical English) and Plain Technical English rules for all English output. Use when writing, reviewing, or rewriting English documentation, instructions, UI text, error messages, commit messages, API docs, or agent responses. Converts complex, verbose, or ambiguous English into short, active, imperative, unambiguous sentences with controlled vocabulary.
---

# Simple English (ASD-STE100 / Plain Technical English)

Apply these rules to ALL English output: documentation, instructions, UI copy, error messages, comments, commit messages, agent replies.

## Directive map

Directives D1–D11 are shared with `simple-russian`. The same ID means the same rule in both languages. Only the examples and the language-specific notes differ. Directives E1–E3 cover features of English only.

| ID | Rule | Scope |
|---|---|---|
| D1 | Imperative for instructions | Shared |
| D2 | Active voice | Shared |
| D3 | Verbs, not nominalizations | Shared |
| D4 | One action per sentence | Shared |
| D5 | Explicit conditions first | Shared |
| D6 | No filler | Shared |
| D7 | One term per concept | Shared |
| D8 | Facts and exact values | Shared |
| D9 | Warnings before actions | Shared |
| D10 | Lists and tables | Shared |
| D11 | Error messages: what, why, what to do | Shared |
| E1 | Noun clusters ≤ 3 words | English only |
| E2 | Simple verb forms, no phrasal verbs | English only |
| E3 | Articles, contractions, word classes | English only |

## Priority of rules

1. Accuracy. Never remove necessary technical information to make a sentence shorter.
2. Clarity. One sentence = one idea. One word = one meaning.
3. Brevity. Remove all words that do not add information.

If a rule below conflicts with accuracy, keep accuracy and restructure the sentence.

## Hard limits

| Parameter | Limit | Directive |
|---|---|---|
| Instruction sentence | ≤ 20 words | D4 |
| Descriptive sentence | ≤ 25 words | — |
| Paragraph | ≤ 6 sentences | — |
| Ideas per sentence | 1 | — |
| Actions per instruction sentence | 1 | D4 |
| Conditions per sentence | 1 | D5 |
| List nesting | ≤ 2 levels | D10 |
| Noun cluster | ≤ 3 words | E1 |
| Verb tense | Simple present, simple past, simple future only | E2 |

Limits are not recommendations. A limit violation is a defect, like a failing test.

---

## Shared directives D1–D11

### D1. Use the imperative for instructions

Start each instruction with a command verb. Do not describe; instruct.

| BAD | GOOD |
|-----|------|
| The file should be saved before you exit. | Save the file before you exit. |
| It is recommended that users restart the service. | Restart the service. |
| You might want to consider checking the logs. | Check the logs. |
| Deletion of the cache can be performed. | Delete the cache. |

If a step is optional, mark it explicitly. Do not use modals for this: "Optional: enable the cache."

### D2. Use the active voice

Name the agent of each action. Passive voice hides the agent and adds words.

| BAD | GOOD |
|-----|------|
| The configuration is loaded by the server at startup. | The server loads the configuration at startup. |
| An error will be shown if the token has expired. | If the token expired, the client shows an error. |
| The tests were made to fail by the migration. | The migration caused the test failures. |

Permitted exception: passive is acceptable in descriptive text when the agent is unknown or unimportant, and the sentence stays short. Example: "The package was deprecated in v3." Never use passive in instructions.

### D3. Use verbs, not nominalizations

A noun made from a verb hides the action. It also needs an empty verb ("perform", "carry out", "make") to carry it.

| BAD | GOOD |
|-----|------|
| Perform a check of the configuration. | Check the configuration. |
| Carry out the installation of the package. | Install the package. |
| Make a decision about the rollback. | Decide whether to roll back. |
| The verification of the build is done by the CI. | The CI verifies the build. |

Search markers: "perform", "carry out", "conduct", "make", "provide" next to a noun that ends in "-tion", "-ment", "-ance", "-al".

### D4. One action per sentence

Split compound instructions. Put steps in a numbered list when order matters.

| BAD | GOOD |
|-----|------|
| Open the terminal, navigate to the project root, and after making sure the environment variables are set, run the build. | 1. Open the terminal. 2. Go to the project root. 3. Make sure the environment variables are set. 4. Run the build. |

Exception: two actions that you cannot separate physically. Example: "Press and hold the button for 3 seconds."

### D5. Make conditions explicit and put them first

Start conditional instructions with "If" or "When". Never hide a condition in the middle or the end of a sentence. Never stack more than one condition per sentence.

| BAD | GOOD |
|-----|------|
| Restart the server, assuming the config changed and unless a deploy is in progress. | If the config changed, restart the server. Do not restart the server during a deploy. |
| The flag enables caching where applicable in most environments. | If the environment supports caching, the flag enables it. Caching is not available in: [list environments]. |

Never use vague conditions: "where applicable", "as needed", "if necessary", "in most cases". State the exact condition or remove it.

### D6. Remove conversational filler

Delete all phrases that carry no information. This is a full prohibition, not a preference.

Prohibited: "please note that", "it is worth mentioning", "as you can see", "basically", "actually", "simply", "just", "feel free to", "obviously", "of course", "in order to" (use "to"), "at this point in time" (use "now"), "due to the fact that" (use "because"), "a great way to", "let's dive into", "I hope this helps".

| BAD | GOOD |
|-----|------|
| Please note that in order to run the tests, you basically just need to execute the command below. | To run the tests, run: |
| It is worth mentioning that this function actually mutates the input. | Caution: this function mutates the input. |

### D7. Use one term for one concept

Choose one term for each concept and use it in the whole document. Do not use synonyms for variety.

| Concept | Use | Do not use |
|---------|-----|-----------|
| begin an action | start | begin, commence, initiate, launch, kick off |
| stop an action | stop | terminate, halt, cease, abort (unless it is the API name) |
| examine | check | verify, inspect, examine, look at (pick "check" unless a distinct meaning is needed) |
| approximately | approximately | around, roughly, about, circa |
| remove | delete / remove | get rid of, eliminate, purge |
| show | show | display, present, render (unless "render" is the technical term) |
| use | use | utilize, leverage, employ |
| make sure | make sure | ensure, guarantee, see to it |

Define every abbreviation at first use, except universally known ones in context (API, URL, HTTP, ID).

Exception: if a word is part of an official API, command, or product name, keep the official name exactly.

### D8. State facts and exact values

Replace every evaluative adjective with a measurement, or delete it.

| BAD | GOOD |
|-----|------|
| very fast | p95 = 40 ms |
| significantly improved | latency −35% |
| large file | file over 100 MB |
| robust solution | 0 failures in 10,000 runs |

- If no metric exists, state the fact without the adjective.
- Keep a space between the number and the unit: `200 ms`, `4 GB`.
- Use a period as the decimal separator and a comma as the thousands separator: `2.5 GB`, `10,000 requests`. In code, commands, and configs, copy the value exactly from the source.
- Reference code as `path/file.ext:line`. Reference sections by exact heading.

### D9. Put warnings and safety notes BEFORE the instruction

| BAD | GOOD |
|-----|------|
| Run the migration script. Note: this drops the temp tables. | Warning: the migration script drops the temp tables. Make a backup first. Then run the migration script. |

Severity levels (the same in all three skills):

| Label | Use for |
|---|---|
| `Warning:` | Risk of injury, loss of data, money, or access, or a security breach |
| `Caution:` | Risk of failure, downtime, degradation, or extra work |
| `Note:` | Useful context without risk |

Never put risk information in a "Note".

### D10. Prefer lists and tables over prose

| Content | Format |
|---|---|
| 3+ steps | Numbered list |
| 3+ items with details (a parameter with a type, a condition with an action) | Bulleted list |
| Items with 2+ attributes each (type, default, required) | Table |
| Up to 3 short plain items (1–3 words each, no details) | Inline is permitted: "JSON, YAML, and TOML" |
| 4+ short plain items | Bulleted list |

- Use a numbered list only when order matters. Otherwise, use a bulleted list.
- Do not nest lists deeper than 2 levels. If you need more levels, use a table or a separate section.

### D11. Error messages: what happened, why, what to do

Formula: what happened → why → what to do. Each part is one short sentence. A button names the action with a verb: "Retry", "Save".

| BAD | GOOD |
|-----|------|
| An error occurred. | Could not save the file: the disk is full. Delete old files and try again. |
| Something went wrong. Try again later. | Payment failed: the bank did not respond in 30 s. Retry the payment. |

---

## English-only directives E1–E3

These rules cover features of English grammar. `simple-russian` has its own language-specific directives (R1–R5).

### E1. Break noun clusters (max 3 words)

English stacks nouns without prepositions. Long noun chains hide the relations between the words. Rewrite with prepositions or split the sentence.

| BAD | GOOD |
|-----|------|
| user session token refresh failure handler | the handler for failures of the session-token refresh |
| database connection pool size configuration parameter | the parameter that sets the size of the connection pool |
| production deployment rollback procedure documentation update | Update the documentation for the rollback procedure. |

### E2. Use simple verb forms and single-word verbs

- Use simple tenses only: simple present, simple past, simple future. Do not use perfect or continuous forms.
- Do not chain gerunds: "after starting the processing of the migrating records" → "after the migration starts".
- Use a single-word verb, not a phrasal verb. A phrasal verb often has several meanings.

| BAD | GOOD |
|-----|------|
| Spin up the cluster. | Start the cluster. |
| Get rid of the temp files. | Delete the temp files. |
| Figure out the cause. | Find the cause. |
| Look into the logs. | Check the logs. |

Exception: keep a phrasal verb if it is the official UI or product term: "Log in", "Sign up".

### E3. Use articles, full forms, and stable word classes

- Write out the article ("the", "a"). Do not drop articles telegraph-style.
- Use "do not", not "don't", in formal documentation. Contractions are acceptable in UI copy and chat.
- Use a verb as a verb and a noun as a noun. Do not use "test" as both in one sentence without markers.

---

## Conversion examples (full passages)

### Example 1 — procedure

BAD:
> Before getting started with the deployment process, it's generally a good idea to make sure that all of the relevant tests have been executed and are passing, after which the deployment can be initiated by the operator via the execution of the deploy script, keeping in mind that the staging environment configuration verification step shouldn't be skipped.

GOOD:
> 1. Run all tests. Make sure they pass.
> 2. Check the staging configuration. Do not skip this step.
> 3. Run the deploy script.

### Example 2 — description

BAD:
> The system is designed in such a way that incoming requests are subjected to validation by the middleware layer, and in the event that validation is not successful, the rejection of the request occurs with the generation of an appropriate error response.

GOOD:
> The middleware validates each incoming request. If validation fails, the middleware rejects the request and returns an error response.

### Example 3 — error message

BAD:
> Oops! Something went wrong while we were trying to process your request. Please try again later or feel free to contact support if the problem persists!

GOOD:
> Request failed: the payment service did not respond (timeout after 30 s). Try again. If the error repeats, contact support with error ID {id}.

## Review checklist

Run this checklist on every English text before you finish.

Limits:
- [ ] Instructions ≤ 20 words. Descriptions ≤ 25 words. Paragraphs ≤ 6 sentences.

Shared directives:
- [ ] D1: Every instruction starts with a command verb. Optional steps are marked "Optional:".
- [ ] D2: No passive voice in instructions.
- [ ] D3: No "perform / carry out / make" + nominalization.
- [ ] D4: One action per instruction sentence.
- [ ] D5: Every condition comes first, starts with "If" or "When", and is exact. One condition per sentence.
- [ ] D6: No filler from the D6 list.
- [ ] D7: One term per concept. Abbreviations defined at first use.
- [ ] D8: No evaluative adjectives. All values exact, with units and correct separators.
- [ ] D9: Warnings and cautions appear before the related instruction, with the correct label.
- [ ] D10: Lists and tables follow the D10 thresholds. Nesting ≤ 2 levels.
- [ ] D11: Each error message states what happened, why, and what to do.

English-only directives:
- [ ] E1: No noun cluster longer than 3 words.
- [ ] E2: Simple tenses only. No gerund chains. No phrasal verbs, except official UI terms.
- [ ] E3: Articles present. No contractions in formal documentation.

Final check:
- [ ] Nothing technically necessary was deleted for brevity.

## Enforcement

- When you WRITE: apply all directives during generation, not after.
- When you REVIEW: quote each violating sentence, name the directive (D1–D11, E1–E3), and give the corrected version in a BAD/GOOD pair.
- When you REWRITE: keep all technical facts. If the source is ambiguous, list the ambiguity as an open question — do not invent a precise value.
- For bilingual work (English + Russian) or translation, also load `simple-lang`. It loads `simple-russian` and adds the parity rules.
