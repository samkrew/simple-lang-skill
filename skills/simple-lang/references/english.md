# English reference: examples and word lists

Use this file with `SKILL.md`. It holds examples, word lists, and rationale for English text. Full before/after passages are in `passages.md`. It never adds or changes a rule. The rules and the limits are in `SKILL.md`.

## Contents

1. Examples by directive
2. Examples for E1–E3
3. Word lists
4. Rationale: differences from ASD-STE100

---

## 1. Examples by directive

### D1. Imperative

| BAD | GOOD |
|-----|------|
| The file should be saved before you exit. | Save the file before you exit. |
| You might want to consider checking the logs. | Check the logs. |
| Deletion of the cache can be performed. | Optional: delete the cache. |

### D2. Active voice

| BAD | GOOD |
|-----|------|
| An error will be shown if the token has expired. | If the token is expired, the client shows an error. |
| The tests were made to fail by the migration. | The migration caused the test failures. |

Example of the D2 exception: "The package was deprecated in v3."

### D3. Verbs, not nominalizations

| BAD | GOOD |
|-----|------|
| Carry out the installation of the package. | Install the package. |
| Make a decision about the rollback. | Decide whether to roll back. |
| The verification of the build is done by the CI. | The CI verifies the build. |

### D4. One idea per sentence

| BAD | GOOD |
|-----|------|
| Open the terminal, navigate to the project root, and after making sure the environment variables are set, run the build. | 1. Open the terminal. 2. Go to the project root. 3. Make sure that the environment variables are set. 4. Run the build. |
| Save the file and restart the service. | Save the file. Then restart the service. |
| The build passed; the artifacts are in `dist/`. | The build passed. The artifacts are in `dist/`. |

### D5. Explicit conditions first

| BAD | GOOD |
|-----|------|
| Restart the server, assuming the config changed and unless a deploy is in progress. | When a deploy is in progress, do not restart the server. If the config changed, restart the server. |
| The flag enables caching where applicable in most environments. | If the environment supports caching, the flag enables it. Caching is not available in: [list the environments]. |
| Clear the cache if necessary. | Optional: clear the cache. |

If the writer knows the condition, the writer states it: "If the build fails with a cache error, clear the cache."

### D6. No filler, answer first

| BAD | GOOD |
|-----|------|
| It is worth mentioning that this function actually mutates the input. | **CAUTION:** This function mutates the input. |
| There are three ways to configure the proxy. | Configure the proxy in one of three ways: |
| Great question! The timeout is in config.yml, I hope this helps. | `config.yml` sets the timeout. |
| Let me check the logs for you. | The logs show 3 timeouts after 10:00. |

### D7. One term for one concept

| BAD | GOOD |
|-----|------|
| Start the worker. When the process is launched, the job initiates. | Start the worker. When the worker starts, the job starts. |
| Set the RTO to 4 h. | Set the recovery time objective (RTO) to 4 h. |

See "D7 vocabulary" in section "3. Word lists".

### D8. Facts, values, and formats

| BAD | GOOD |
|-----|------|
| significantly improved | latency −35% |
| large file | file over 2 GB |
| robust solution | 0 failures in 10,000 runs |
| 1000 requests | 1,000 requests |
| port 50,051 | port 50051 |
| 10/09/2026 | 9 October 2026, or `2026-10-09` |

### D9. Warnings before actions

| BAD | GOOD |
|-----|------|
| Run the migration script. Note: this drops the `audit_log` table. | **WARNING:** The migration script drops the `audit_log` table. Make a backup first. Then run the script. |
| Restart the service (this interrupts active sessions). | **CAUTION:** A restart interrupts active sessions for 30 s. Tell the on-call engineer. Then restart the service. |

### D10. Lists and tables

BAD:

> The script takes several parameters: first, the config path, which is required, second, a verbose flag, which is optional, and finally a timeout in seconds that defaults to thirty.

GOOD:

> Script parameters:
>
> | Parameter | Required | Default |
> |---|---|---|
> | `--config` (config path) | yes | — |
> | `--verbose` (detailed output) | no | off |
> | `--timeout` (seconds) | no | 30 |

### D11. Error messages and UI text

| BAD | GOOD |
|-----|------|
| Something went wrong. Try again later. | Payment failed: the bank did not respond in 30 s. Retry the payment. |
| [OK] [Cancel] on a delete dialog | [Delete] [Cancel] |

### D12. Keep modality

| Draft | Wrong edit | Correct edit |
|---|---|---|
| The outage was probably caused by the cert rotation. | The cert rotation caused the outage. | The cert rotation probably caused the outage. |
| The fix should resolve most timeouts. | The fix resolves the timeouts. | The fix will probably resolve most timeouts. |

---

## 2. Examples for E1–E3

### E1. Noun clusters

| BAD | GOOD |
|-----|------|
| user session token refresh failure handler | the handler that runs when the refresh of a user session token fails |
| production deployment rollback procedure documentation update | Update the documentation for the rollback procedure of production deployments. |

### E2. Verb forms

| BAD | GOOD |
|-----|------|
| The team has fixed the bug. | The team fixed the bug. |
| We are seeing errors in the logs. | The logs show errors. |
| After starting the processing of the migrating records… | After the processing of the migrating records starts… |

Permitted present perfect (a state that continues now): "The service has not responded since 10:00."

Permitted technical phrasal verbs (no single-word equivalent): "back up", "roll back".

### E3. Articles and word classes

| BAD | GOOD |
|-----|------|
| Open config, set timeout, restart service. | 1. Open the config. 2. Set the timeout. 3. Restart the service. |
| Don't edit the lock file. (in a runbook) | Do not edit the lock file. |
| Test the new test. | Run the new test. |

---

## 3. Word lists

### D3 markers

Empty verbs: "perform", "carry out", "conduct", "make", "provide", "execute". Action nouns that often follow them: "installation", "deployment", "verification", "maintenance", "removal", "decision", "analysis".

### D6 filler

Delete: "please note that", "it is worth mentioning", "as you can see", "basically", "actually", "simply", "just", "feel free to", "obviously", "of course", "a great way to", "let's dive into", "I hope this helps".

Delete intensifiers: "very", "really", "extremely", "highly".

Replace: "in order to" → "to", "at this point in time" → "now", "due to the fact that" → "because".

### D7 vocabulary

| Concept | Use | Do not use |
|---------|-----|-----------|
| begin an action | start | begin, commence, initiate, launch, kick off |
| stop an action | stop | terminate, halt, cease, abort (unless it is the API name) |
| examine | check | inspect, examine, look at ("verify" only for a formal verification step) |
| approximately | approximately | around, roughly, about, circa |
| take data away | delete | get rid of, eliminate, purge |
| take an item out of a set | remove | eliminate, drop (unless it is the API name) |
| show | show | display, present, render (unless "render" is the technical term) |
| use | use | utilize, leverage, employ |
| make sure | make sure | ensure, guarantee, see to it |
| come after | follow | — ("follow" means "come after". For instructions, write "obey".) |

### E2 phrasal verbs

| Phrasal verb | Single-word verb |
|---|---|
| spin up | start |
| shut down | stop (keep `shutdown` if it is the command) |
| set up | configure, install |
| get rid of | delete |
| figure out, find out | find |
| look into | check |
| go through | read, check |
| carry out + an action noun | the verb of that noun: "carry out the installation" → "install" (D3) |

---

## 4. Rationale: differences from ASD-STE100

ASD-STE100 (Simplified Technical English, STE) is the base of the English rules. The skill does not implement the full standard:

- The skill does not use the STE dictionary of approved words. D7 and the word lists in section "3. Word lists" cover frequent cases only.
- STE permits only three tenses: the simple present, the simple past, and the simple future. E2 also permits the present perfect for a state that continues now. E2 and D12 keep a hedge in its source form, for example "may have failed".
- STE permits an -ing form only as a modifier in a technical name. E2 also permits -ing nouns ("logging") and -ing adjectives ("incoming request").
- STE does not permit phrasal verbs. E2 permits technical verbs without a single-word equivalent ("back up", "roll back") and official UI terms.
- STE documentation does not use contractions. E3 permits them in UI copy and chat.
- STE permits each approved word only as the part of speech that its dictionary gives. E3 forbids two parts of speech for one word only inside one sentence.
- In STE, WARNING means a risk of injury or death, and CAUTION means a risk of damage to equipment. Software text seldom has these risks. So D9 also gives the WARNING level to a security breach and to an irreversible loss of data, money, or access.
- STE starts a safety instruction with a command or a condition. Then it explains the risk and its possible results. D9 gives the risk first, so the reader knows the risk before the action.
