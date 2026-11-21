# Outcome prioritization for WHO guidelines

A short survey and a dashboard used to ask a guideline development group (GDG) to rate the importance of candidate outcomes, following the [Core GRADE](https://doi.org/10.1136/bmj-2024-081903) approach. First used for the **filovirus disease (FVD) therapeutics guideline**.

| Folder | What it is |
|---|---|
| `survey/` | The survey, ready to import into the WHO dataform (LimeSurvey 6) |
| `report/` | A dashboard that summarises the ratings, with an Excel download |

## How it works

GDG members were invited by personal link to rate each outcome from 1 to 9 (1–3 of limited importance; 4–6 important; 7–9 critically important), following the Core GRADE approach.

1. **Each GDG member receives a personal email** explaining the 1–9 scale (1–3 limited importance, 4–6 important, 7–9 critically important) and a *Rate the outcomes* button.
2. **One page** lists all outcomes with a 1–9 score for each, and an optional comments box. Members can reopen their link to change their ratings until the survey closes.
3. **The report** shows, for each outcome:
   - the median score and interquartile range, and the GRADE tier from the median;
   - the share of raters scoring 1–3, 4–6 and 7–9, and whether there is consensus (by default at least 70% in a tier and fewer than 15% in the opposite tier);
   - every individual score, with raters anonymised (Rater 01, 02 …);
   - the comments.

## Using the survey

1. In the dataform: **Create survey → Import** → `survey/FVD_outcome_prioritization.lss`.
2. Edit the outcome list if needed (keep the codes `O01`, `O02` … and update the list at the top of the report to match).
3. **Participants**: import a CSV with `firstname, lastname, email, language`.
4. Fill in the deadline in the email templates, activate the survey, and send the invitations from **Participants → Invitations & reminders**.

## Running the report

Requirements: [R](https://cran.r-project.org/) and [Quarto](https://quarto.org/), with the R packages
`data.table, plotly, DT, htmltools, bslib, openxlsx, jsonlite`.

1. Export from the dataform into `report/data/`:
   - **Responses → Export → CSV** (all responses) → `results-survey<surveyID>.csv`
   - optional: **Participants → Export → CSV** → `tokens_<surveyID>.csv` (for the response rate)
2. Delete the dummy files `results-survey000000.csv` and `tokens_000000.csv`.
3. From the `report/` folder:

   ```bash
   quarto render outcome_prioritization_report.qmd
   ```

   This produces `outcome_prioritization_report.html` (the dashboard, with the Excel file downloadable from it) and `FVD_outcome_prioritization_<date>.xlsx`.

Options (add `-P name:value`):

| Option | Default | Use |
|---|---|---|
| `show_personal` | `true` | `false` leaves out the comments and the Excel download |
| `exclude_tokens` | none | Access codes of test replies to leave out |
| `consensus_in_tier` / `consensus_max_opposite` | 70 / 15 | Consensus rule, if the GDG agrees another one |

## Data protection

Real exports and rendered outputs are never committed: `.gitignore` keeps everything in `report/data/` out of the repository except the dummy files, and ignores all `.html` and `.xlsx` files. The only exception is a public version of the report, named `*_public.html` (see below).

## Publishing on GitHub

First time (create an empty repository called `fvd-outcome-prioritization` on GitHub, with no README or licence):

```bash
cd path/to/fvd-outcome-prioritization
git init
git add .
git status        # check: no real results-survey / tokens files, no .xlsx, no full .html report
git commit -m "Outcome prioritization: survey and report"
git branch -M main
git remote add origin https://github.com/WHE-HQ/fvd-outcome-prioritization.git
git push -u origin main
```

To share a version of the report without comments (for example on GitHub Pages), from `report/`:

```bash
quarto render outcome_prioritization_report.qmd -P show_personal:false -o outcome_prioritization_public.html
cd ..
git add report/outcome_prioritization_public.html
git status        # check: only the _public.html file is listed
git commit -m "Update public report"
git push
```

The full report (`outcome_prioritization_report.html`) and the Excel file stay ignored. Never add them with `git add -f`.

To turn on GitHub Pages: **Settings → Pages → Deploy from a branch → main / (root)**. The public report is then at
`https://whe-hq.github.io/fvd-outcome-prioritization/report/outcome_prioritization_public.html`.

## Contact

Antoine Chaillon – chaillona@who.int
WHO Clinical Management & Operations

## Licence

© World Health Organization 2026. Available under CC BY-NC-SA 3.0 IGO – see [LICENSE](LICENSE). The WHO logo is not covered by the licence.
