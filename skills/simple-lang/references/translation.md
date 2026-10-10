# Translation and parity (EN ↔ RU)

Use this file with `SKILL.md` and the reference of the target language. It holds the translation procedure, common traps, and the parity contract. All other rules and all limits are in `SKILL.md`.

## Procedure

1. Reduce each source sentence to its skeleton: condition + actor + verb + object.
2. Map each term 1:1 through the glossary of the document (D7). Do not use synonyms.
3. Translate the skeleton, not the sentence. Write it with the rules of the target language: D-rules plus E1–E3 or R1–R5.
4. Check the target text against the checklist in `SKILL.md`. Use the limits of the target language.
5. Check the parity contract below.

## Traps

| Trap | Wrong | Correct |
|---|---|---|
| EN passive → RU passive | "is deleted by the script" → «удаляется скриптом» | «скрипт удаляет» (D2) |
| RU nominalization → EN nominalization | «выполнение проверки» → "the performing of the check" | "check" / «проверьте» (D3) |
| EN modal → RU «следует» | "should restart" → «следует перезапустить» | "restart" / «перезапустите» (D1) |
| RU «необходимо» → EN "it is necessary" | «необходимо остановить» → "it is necessary to stop" | "stop" / «остановите» (D1) |
| Politeness inflation | "please kindly ensure" ↔ «пожалуйста, убедитесь, что» | "make sure" / «убедитесь» (D6) |
| Value invented | "several seconds" → «≤ 5 с» | «несколько секунд» and an open question to the author. If the source gives the value, use it in both versions: "≤ 5 s" / «≤ 5 с» |
| Number format copied | "2.5 GB" → «2.5 ГБ», "10,000" → «10,000» | «2,5 ГБ», «10 000» (D8) |
| Percent copied | "35%" → «35%» | «35 %» (D8) |
| Date order copied | "10/09/2026" (US: October 9) → «10/09/2026» (a Russian reader sees 10 September) | «9 октября 2026 г.» or `2026-10-09` (D8) |
| Quotes copied | "Save" → "Сохранить" | «Сохранить» (D8) |
| Label level shifted | "**WARNING:**" → «**ВНИМАНИЕ:**» | «**ОСТОРОЖНО:**» (level 1 = level 1, D9) |
| Hedge lost | "may have failed" → «завершилось с ошибкой» | «возможно, завершилось с ошибкой» (D12) |
| EN of-chain → RU genitive chain | "the size limit of the connection pool" → «лимит размера пула соединений» | «лимит на размер пула соединений» (R2) |
| EN participle clause → RU деепричастие | "Run the script, passing `--dry-run`" → «Запустите скрипт, передав `--dry-run`» | «Запустите скрипт с флагом `--dry-run`» (R4) |
| Aspect in a prohibition | "Do not open the file" → «Не откройте файл» | «Не открывайте файл» (R5) |
| Command form | "Open the file" in documentation → «Открыть файл» / «Открой файл» | «Откройте файл» (R5) |
| Button form | "Save" → «Сохраните» | «Сохранить» (D11) |
| Title Case copied | "Save Changes" → «Сохранить Изменения» | «Сохранить изменения» (D11) |

## Parity contract

A document with EN and RU versions passes parity when all of these are true:

- Both versions have the same actions in the same order. The number of sentences can differ, because the sentence limits differ.
- Both versions have the same warnings, with the same levels and in the same positions.
- Each value is the same in both versions. Each version uses the separators, unit symbols, and date format of its language (D8): "2.5 GB" = «2,5 ГБ».
- Commands, code, paths, identifiers, and config values are byte-identical.
- The hedges match (D12).
- Neither version has content that the other version does not have.

If parity fails, the verdict is BLOCK (`SKILL.md`, Enforcement). Change the translation to match the source version. Do not average the two versions. If you do not know which version is the source, list each difference as an open question.
