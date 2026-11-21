# Data folder

Put the exports from the dataform here before running the report:

- `results-survey<surveyID>.csv` – **Responses → Export → CSV** (all responses) – required
- `tokens_<surveyID>.csv` – **Participants → Export → CSV** – optional; gives the number invited and the response rate

The newest file of each kind is used.

`results-survey000000.csv` and `tokens_000000.csv` are **dummy files** (invented raters) so the report can be tried straight away. Delete them when you use real exports.

Real exports are excluded from the repository by `.gitignore`. Do not change that.
