# Swiss psychotherapy billing: tariff rules, invoices, FODA-XML export

Service capture and billing for a Swiss psychotherapy practice, part of the practice app I built. Therapists enter minutes; the app works out tariff code, quantity and price.

This is a write-up. The source code belongs to the practice and is not public.

## The domain

Psychological psychotherapy in Switzerland is billed under tariff 581 (compulsory health insurance) and tariff 582 (disability and accident insurance, IV/SUVA). Which code applies depends on payer, duration, setting (individual or group) and whether the therapist is still in training. Preparation time is billed separately from session time. By hand this is slow, and mistakes are easy to make.

## What the app does

- **Rule engine.** From minutes, payer and training status it derives tariff code and quantity, for example PA010 under compulsory insurance or 582.2 under IV/SUVA. Cases without a match, such as self-payers or minute values that do not fit the IV/SUVA grid, get a clear hint and a manual choice.
- **Quick capture.** After a session, session and preparation time are pre-filled with the usual minutes and only need confirming.
- **Blocker list.** Everything that stops billing in one list: missing service, missing session note, missing referral, report awaiting approval. Each entry jumps to the session and can become a task.
- **Approval tracking.** Session counters per case. After 15 sessions the app reminds the therapist to check the referral, after 30 that cost approval from the insurer is needed.
- **Invoices.** Choosing a tariff position fills quantity, price and text; group sessions take the number of participants. PDF output.
- **Export.** Invoices in the Swiss FODA-XML 5.0 format, validated before export.

## Tests

22 test cases with fixed expected results cover the rule engine and the export checks. Tariff rules are the kind of code where a test with a known answer is worth more than any review.

## Stack

Node.js · Express · SQLite · pdfkit · XML · React
