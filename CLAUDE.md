# CLAUDE.md: Maligaya Sales System

## What this is
A single-file web app (`index.html`) that turns the owner's pasted daily sales notes into a printable monthly report.
Runs entirely in the browser. No build step, no server, no dependencies. Hosted on GitHub Pages
(https://omeoproject.github.io/maligaya-sales/).

## Who uses it
A small shop owner who reports sales on the 1st of every month. Not a developer. Keep the UI plain,
big buttons, plain English. Currency is Philippine peso (PHP).

## Files
- `index.html`: the whole app (HTML + CSS + JS in one file). Keep it that way.
- `README.md`: short public description.

## Data privacy (important)
- The repo and the live site are PUBLIC. Never commit real sales, salary, employee or owner data.
- `const SEED` in `index.html` must stay `{"months":{}}`. Real data is loaded by the owner through "Load backup".
- Records live only in the user's browser `localStorage` (key `maligaya_sales_v1`). Never add code that uploads data anywhere.
- Don't add analytics, trackers, or external scripts/fonts.

## Input format (the pasted notes)
```
Date: october 3,2026
Cash: 1150
Gcash: 0
Bank: 0
--------------------
Total sales: 1150
Expenses: 75 + 235 comi
```
- Weekly block: a line `---`, then `Weekly sales`, then a range line like `October 3- 11`.
- Salary block: `Salary: oct 3-10`, then `Name- 8x400 =3200` lines (amount = the numbers after the last `=`).
- Abono block: a line containing `abono`, then `Name - amount` lines.
- Taglish words appear in notes (`comi`, `abono`, `pagcash nalang po...`). Ignore text that is not a recognised label.

## Rules the parser follows (don't change without asking)
- Sales = Cash + GCash + Bank. `Total sales` in the notes is only used to warn on a mismatch.
- Expenses = sum of every number in the Expenses line, except an amount followed by the word `salary`.
- Pasting a day that already exists replaces it. Salary/weeks are replaced for the month when present in the paste.
- Weeks come from the `Weekly sales` ranges; with none, Monday-Sunday weeks are used.
- "Abono" is shown in English as "Owner contributions (abono)" everywhere the user sees it.

## Report layout (keep stable, the owner prints it)
Monthly summary, weekly summary (with net before abono), owner contributions (abono), salary breakdown,
daily log, signature lines. A4, prints cleanly in black and white. Negative numbers shown with a minus sign.
Report must always say which days did not match their stated total.

## How to work on it
- Test by opening `index.html` in a browser; check the browser console for errors.
- After any parser change, re-test with a real-format sample: totals must equal the sum of the daily rows,
  weekly rows must add up to the month row, and a mismatching day must show a warning.
- Never make an edit that changes past months' totals without telling the user.

## Style
- Plain JavaScript (no framework), `"use strict"`, small functions. Arial font. No emoji in the UI.
- Escape any user text inserted into HTML (`esc()`).

## Do not
- Do not add a login, backend, or database.
- Do not give financial advice; the app only totals what was entered.
- Do not guess missing numbers. Show a warning and let the user fix it in Records.
