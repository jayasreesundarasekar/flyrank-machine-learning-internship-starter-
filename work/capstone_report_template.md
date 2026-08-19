# Capstone Report

**Author:** Jayasree  
**Lane:** ML-07 — Content Action Scoring  
**Repo:** `[https://github.com/jayasreesundarasekar/flyrank-machine-learning-internship-starter-]`  
**Date:** 2026-08-20

> **Note:** Replace all illustrative metrics and lane-specific details with results from the executed notebooks before presenting this as measured research.

## 0. Abstract

This study asks whether historical search-performance signals can help prioritize content opportunities for human review. The analysis uses the FlyRank ML Internship dataset and focuses on historical observations available within the selected development window. I compared a transparent rule-based baseline with a Logistic Regression model using a time-aware validation design and leakage checks. In the illustrative evaluation, the model measured higher F1 performance than the baseline on the same validation split. The resulting ranked queue is intended as directional decision-support for editors rather than an automated content decision system.

## 1. Problem framing

The decision supported by this analysis is which content opportunities should be reviewed first.

The unit of analysis is one **lane-specific observation** over the relevant time period. The output is a ranked action score with a reason code and recommended action.

A FlyRank editor can use the ranking to prioritize pages or opportunities for investigation rather than reviewing every observation equally.

A wrong call can waste editorial effort or lead to an unnecessary content change. Data and ML can help by consistently prioritizing observations using measurable historical signals, while human review remains responsible for the final decision.

## 2. Data safety

The analysis uses the FlyRank ML Internship dataset and a mid-panel development window. The final outcome window was kept separate from feature development.

I deliberately excluded label-derived fields such as `trend_direction` and `trend_pct` because they can contain information derived from the outcome being predicted. I also excluded pseudonymous identifiers from the model features; they may be used for grouping or validation but are not treated as predictive signals.

Future-window measurements were excluded because they would not be available at decision time.

No client-identifying information is intentionally included in the analysis, notebook outputs, or public-facing report.

## 3. Baseline

The Week-4 baseline is a transparent rule-based action score built from historical signals.

The rule gives higher priority to observations showing stronger evidence from the selected signals and assigns one reason code explaining the primary recommendation.

The baseline is a fair comparison because the model is evaluated on the same observations, target definition, split, and metric.

| Method | F1 |
|---|---:|
| Week-4 baseline | 0.61 |
| Logistic Regression | 0.68 |

The model therefore measured a **0.07 absolute F1 improvement** over the baseline in this illustrative evaluation.

## 4. Model / analysis

I used Logistic Regression because the target is binary and the method provides an interpretable model without unnecessary complexity.

The illustrative feature set contains:

1. Historical search volume
2. Historical CTR
3. Historical average position
4. Recent historical clicks
5. Recent historical impressions

I deliberately left out future-window values, label-derived variables, client-identifying fields, and product flags that could leak the outcome.

**Target definition:** The target represents the selected historical outcome used to evaluate whether an observation should receive the modeled action priority.

## 5. Evaluation

I used a time-aware split, with earlier observations used for training and later observations used for validation. This better reflects the intended use because the model would make decisions using information available before the future outcome.

### Model vs baseline

| Method | F1 |
|---|---:|
| Week-4 baseline | 0.61 |
| Week-5 model | 0.68 |

The illustrative majority-class base rate was **64%**, which is reported alongside the model metric to provide context.

The model's errors were concentrated around borderline observations where the available historical signals did not clearly separate the two outcome classes. Some high-scoring observations were also false positives, showing why the ranking should not be treated as an automatic decision.

## 6. Interpretation

The model placed the greatest weight on historical search-performance signals rather than identifiers or future information.

The strongest observed signals were historical volume and CTR-related measurements. This is directionally consistent with the purpose of prioritizing content opportunities.

A useful negative result is that a higher model score does not guarantee that a recommended action will produce a better future outcome. The analysis measures predictive association in the evaluated data rather than causal impact.

The model therefore provides prioritization support rather than proof that changing a page will improve its performance.

## 7. Recommendation

The ranked output supports three levels of editorial attention:

1. **High-priority review** — investigate first because multiple historical signals indicate a potentially valuable opportunity.
2. **Review** — investigate when the evidence is moderate or mixed.
3. **Monitor** — retain for observation when the evidence is weak.

Each recommendation has one reason code so that an editor can understand why the observation was ranked.

A FlyRank editor could use the queue as a triage list: review the highest-ranked opportunities first, inspect the underlying search context, and decide manually whether an action is appropriate.

Confidence is limited by the available historical data, model performance, and validation design. The recommendations are directional and require human review.

## 8. Reproducibility

The analysis is implemented in the repository under `work/notebooks/`.

Relevant notebooks:

- `work/notebooks/w03_data_contract.ipynb`
- `work/notebooks/w04_baseline_score.ipynb`
- `work/notebooks/w05_model.ipynb`
- `work/notebooks/w06_validation_audit.ipynb`
- `work/notebooks/w07_action_playbook.ipynb`
- `work/notebooks/capstone.ipynb`

### Re-run

```bash
git clone [https://github.com/jayasreesundarasekar/flyrank-machine-learning-internship-starter-]
cd [flyrank-machine-learning-internship-starter-]

pip install -r requirements.txt

jupyter notebook