---
name: simple-english
description: Enforce ASD-STE100 (Simplified Technical English) and Plain Technical English rules for all English output. Use when writing, reviewing, or rewriting English documentation, instructions, UI text, error messages, commit messages, API docs, or agent responses. Converts complex, verbose, or ambiguous English into short, active, imperative, unambiguous sentences with controlled vocabulary.
---

# Simple English (ASD-STE100 / Plain Technical English)

Apply these rules to ALL English output: documentation, instructions, UI copy, error messages, comments, commit messages, agent replies.

## Priority of rules

1. Accuracy. Never remove necessary technical information to make a sentence shorter.
2. Clarity. One sentence = one idea. One word = one meaning.
3. Brevity. Remove all words that do not add information.

If a rule below conflicts with accuracy, keep accuracy and restructure the sentence.

## Core constraints (HARD LIMITS)

| # | Constraint | Limit |
|---|-----------|-------|
| 1 | Sentence length — procedural (instructions) | ≤ 20 words |
| 2 | Sentence length — descriptive (explanations) | ≤ 25 words |
| 3 | Paragraph length | ≤ 6 sentences |
| 4 | Noun cluster length | ≤ 3 words |
| 5 | Ideas per sentence | 1 |
| 6 | Instructions per sentence | 1 |
| 7 | Verb voice in instructions | Active, imperative |
| 8 | Verb tense | Simple present, simple past, simple future only |

## Operational directives

### D1. Use the imperative for instructions

Start each instruction with a command verb. Do not describe; instruct.

| BAD | GOOD |
|-----|------|
| The file should be saved before you exit. | Save the file before you exit. |
| It is recommended that users restart the service. | Restart the service. |
| You might want to consider checking the logs. | Check the logs. |
| Deletion of the cache can be performed. | Delete the cache. |

### D2. Use the active voice

Name the agent of each action. Passive voice hides the agent and adds words.

| BAD | GOOD |
|-----|------|
| The configuration is loaded by the server at startup. | The server loads the configuration at startup. |
| An error will be shown if the token has expired. | If the token expired, the client shows an error. |
| The tests were made to fail by the migration. | The migration caused the test failures. |

Permitted exception: passive is acceptable in descriptive text when the agent is unknown or unimportant, and the sentence stays short. Example: "The package was deprecated in v3."

### D3. One instruction per sentence

Split compound instructions. Put steps in a numbered list when order matters.

| BAD | GOOD |
|-----|------|
| Open the terminal, navigate to the project root, and after making sure the environment variables are set, run the build. | 1. Open the terminal. 2. Go to the project root. 3. Make sure the environment variables are set. 4. Run the build. |

### D4. Make conditions explicit and put them first

Start conditional instructions with "If" or "When". Never hide a condition in the middle or the end of a sentence. Never stack more than one condition per sentence.

| BAD | GOOD |
|-----|------|
| Restart the server, assuming the config changed and unless a deploy is in progress. | If the config changed, restart the server. Do not restart the server during a deploy. |
| The flag enables caching where applicable in most environments. | If the environment supports caching, the flag enables it. Caching is not available in: [list environments]. |

Never use vague conditions: "where applicable", "as needed", "if necessary", "in most cases". State the exact condition or remove it.

### D5. Break noun clusters (max 3 words)

Long noun chains hide the relations between the words. Rewrite with prepositions or split the sentence.

| BAD | GOOD |
|-----|------|
| user session token refresh failure handler | the handler for failures of the session-token refresh |
| database connection pool size configuration parameter | the parameter that sets the size of the connection pool |
| production deployment rollback procedure documentation update | Update the documentation for the rollback procedure. |

### D6. Remove conversational filler

Delete all phrases that carry no information. This is a full prohibition, not a preference.

Prohibited: "please note that", "it is worth mentioning", "as you can see", "basically", "actually", "simply", "just", "feel free to", "obviously", "of course", "in order to" (use "to"), "at this point in time" (use "now"), "due to the fact that" (use "because"), "a great way to", "let's dive into", "I hope this helps".

| BAD | GOOD |
|-----|------|
| Please note that in order to run the tests, you basically just need to execute the command below. | To run the tests, run: |
| It is worth mentioning that this function actually mutates the input. | Warning: this function mutates the input. |

### D7. Use one word for one meaning (controlled vocabulary)

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

Exception: if a word is part of an official API, command, or product name, keep the official name exactly.

### D8. Use approved word forms

- Use "do not", not "don't", in formal documentation. Contractions are acceptable in UI copy and chat.
- Use a verb as a verb and a noun as a noun. Do not use "test" as both in one sentence without markers.
- Write out the article ("the", "a") — do not drop articles telegraph-style.
- Define every abbreviation at first use, except universally known ones in context (API, URL, HTTP, ID).

### D9. Warnings and safety notes come BEFORE the instruction

| BAD | GOOD |
|-----|------|
| Run the migration script. Note: this drops the temp tables. | Warning: the migration script drops the temp tables. Make a backup first. Then run the migration script. |

Format: `Warning:` for risk of damage or data loss. `Note:` for helpful context only. Never put risk information in a "Note".

### D10. Prefer lists and tables over prose

- Sequence of steps → numbered list.
- Set of options, parameters, or conditions → table or bulleted list.
- Maximum 3 items enumerated inline in a sentence; 4+ items → list.

### D11. Numbers, units, and references

- Write exact values. "Fast" → "under 200 ms". "Large file" → "over 100 MB".
- Keep a space between the number and the unit: `200 ms`, `4 GB`.
- Reference code as `path/file.ext:line`. Reference sections by exact heading.

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

Run this checklist on every English text before you finish:

- [ ] Every instruction sentence ≤ 20 words; descriptive ≤ 25 words.
- [ ] Every instruction starts with a command verb (imperative).
- [ ] No passive voice in instructions.
- [ ] One idea per sentence; one instruction per sentence.
- [ ] Every condition starts with "If" or "When" and is exact — no "as needed", "where applicable".
- [ ] No noun cluster longer than 3 words.
- [ ] No prohibited filler words (D6 list).
- [ ] One term per concept across the whole text (D7 table).
- [ ] Warnings appear before the related instruction, marked `Warning:`.
- [ ] 4+ inline items converted to a list; ordered steps numbered.
- [ ] All values exact, with units; all abbreviations defined at first use.
- [ ] Nothing technically necessary was deleted for brevity.

## Enforcement

- When you WRITE: apply all directives during generation, not after.
- When you REVIEW: quote each violating sentence, name the directive (D1–D11), and give the corrected version in a BAD/GOOD pair.
- When you REWRITE: keep all technical facts. If the source is ambiguous, list the ambiguity as an open question — do not invent a precise value.
- For bilingual work (English + Russian), also load `simple-russian` and `simple-lang`.
