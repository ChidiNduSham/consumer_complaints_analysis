# Consumer Complaint Patterns and Response Trends

Exploratory data analysis and statistical investigation of **62,516 CFPB consumer complaints about Bank of America** (2017-05-01 to 2023-08-28). Built for Week 4 of the AnalystLab Africa Experience Lab internship programme (Data Science track).

> A complaint is an issue reported by a consumer. It does not establish misconduct or prove a regulatory breach, and a recorded company response is not proof that a complaint was resolved.

## What this project does

* Inspects and documents the quality of the data (14 logged checks, no rows deleted, no values invented).
* Explores complaint volumes (annual and monthly), products, issues, company responses, timely-response rates and CFPB forwarding intervals, with 12 charts.
* Tests three hypotheses with effect sizes and robustness checks.
* States findings, limitations, alternative explanations and recommendations.

## Key results

| Area | Result |
|---|---|
| Volume | 7,872 (2018) rising to 12,953 (2022); 2023 is the highest year like-for-like (1 Jan to 28 Aug) |
| Concentration | Checking or savings account: 39.7% of complaints; top four products: 88.5% |
| Top issue | "Managing an account": 24.2% of complaints |
| Responses | 65.7% closed with explanation; 23.5% monetary relief; 8.4% non-monetary relief; 1,494 in progress |
| Timely response | 96.1% of 61,022 valid records; "No" rose from about 0.1% (2018–19) to 11.0% (2021) |
| Forwarding interval | 79.4% forwarded the same day; median 0 days, mean 1.22, max 275 |
| H1 | Intervals differ by product (Kruskal–Wallis, p < 0.001) but the effect is small (ε² = 0.024) and largely a channel-mix effect |
| H2 | Response mix is associated with product (χ², p < 0.001, V = 0.21) |
| H3 | Timely-response share changed across years (χ², p < 0.001, V = 0.21) |

See `Analytical_Report.md` for the full write-up.

## Method in brief

1. **Inspect:** structure, dictionary match, Complaint ID uniqueness, duplicates, missing values, dates, categories.
2. **Prepare:** parse dates; derive year, month, quarter, forwarding interval and interval bands; label blanks; flag unusual values; save a cleaned copy (the original file is never modified, and its hash is re-checked).
3. **Explore:** counts and shares, trends, product-by-issue, response mix, timeliness, interval distribution.
4. **Test:** Kruskal–Wallis (H1) and chi-square tests of independence (H2, H3), each with effect size (ε² or Cramér's V), assumption checks and a robustness check.
5. **Validate:** 13 post-cleaning checks (row counts, totals reconcile, dates, hashes).

## Setup and reproduction

Requires Python 3.10+ (developed on 3.13).

```bash
git clone <your-repository-url>
cd <your-repository-name>
python -m venv .venv && source .venv/bin/activate     # Windows: .venv\Scripts\activate
pip install pandas numpy matplotlib seaborn scipy jupyter
# put the two supplied data files in ./data/ (see structure above)
jupyter notebook Shammah Ndubuisi Submission_Week4_Consumer_Complaints_Analysis.ipynb   # restart kernel, run all cells
```

The Excel worksheet `Data` must be exported to `data/Consumer_Complaints.csv` (the supplied CSV export is used as-is). The analysis is deterministic; a seed (42) is set for completeness.

## Definitions and assumptions

* **Complaint volume:** rows counted by `Date submitted` (one row per Complaint ID).
* **Forwarding interval:** `Date received` minus `Date submitted`, in days. This is the CFPB-to-company interval, **not** the company's response or resolution time.
* **Timely-response %:** Yes ÷ (Yes + No). Blank values (all "In progress") are excluded and reported separately, never counted as "No".
* **Partial periods:** 2017 (May–Dec), 2023 (Jan–28 Aug) and August 2023 are incomplete; like-for-like comparisons use 1 Jan–28 Aug.
* **Assumptions:** "Web Referral" is treated as "Referral" for channel groupings; the three smallest products are combined for tests; "unusually long" interval means over 30 days.

## Limitations

Complaints are consumer reports, not verified facts; counts are not rates (no denominator); data covers one company and a partial period; complaints from one consumer cannot be linked, so independence cannot be checked; channel, product and issue mix change together; "timely" and relief categories are not defined in the data; there are no narratives. Full discussion in the report.

## Data source and attribution

Consumer Complaints on Financial Products and Services (Bank of America, 2017–2023), provided by the Consumer Financial Protection Bureau (CFPB) via AnalystLab Africa. The data contains no names, narratives or personal identifiers; only the consumer's state is recorded. This project is not an official CFPB or Bank of America product.
