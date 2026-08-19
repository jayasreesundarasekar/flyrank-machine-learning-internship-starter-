# Capstone Report — Search Intelligence Content Opportunity Scoring

**Author:** Jayasree  
**Lane:** Refresh / Content Opportunity Scoring  
**Repo:** [YOUR REPOSITORY URL]  
**Date:** 2026-08-20

> **Research note:** This capstone is a research and decision-support artifact. Results are described using observed, measured, directional, and decision-support language. Replace bracketed values with the actual outputs from the executed notebooks before publication.

---

## 0. Abstract

This study asks whether historical search-performance signals can help prioritize content opportunities for human review. The analysis uses the FlyRank ML Internship dataset and focuses on historical observations from the selected development window while excluding future-window and label-derived information. A transparent rule-based baseline is compared with a machine-learning model using a time-aware validation design and explicit leakage checks. The evaluated model measured **[MODEL METRIC]** compared with **[BASELINE METRIC]** on the same validation data and metric. The resulting ranked queue is intended to help editors prioritize content reviews as directional decision-support rather than automatically determine content changes.

---

## 1. Problem framing

### Decision being supported

The decision supported by this project is:

> **Which content opportunities should be reviewed first based on historical search-performance signals?**

The purpose is to help a human editor prioritize investigation rather than automate editorial decisions.

### Unit of analysis

The unit of analysis is a **page-level search observation over a defined historical time window**.

Each observation represents historical search-performance signals available for that page during the relevant observation period.

### Output

The system produces:

- a priority score
- a ranked queue
- a reason code
- an action recommendation

The score is intended to prioritize human attention.

### Human action

A FlyRank editor can use the ranked queue to identify which observations should be investigated first.

The editor should inspect the underlying context before deciding whether to:

- improve content
- review metadata
- investigate a decline
- monitor performance
- or take no action

### Cost of a wrong call

A wrong recommendation can waste editorial time or lead to an unnecessary content change.

The system therefore prioritizes review rather than making irreversible decisions automatically.

### Why data and ML help

A search-content team may have more potential opportunities than can be manually reviewed in the same amount of time.

Historical search signals provide a consistent way to prioritize those opportunities.

Machine learning can help identify combinations of signals that are difficult to rank manually, while a transparent rule-based baseline provides a simple benchmark.

The purpose is prioritization and decision-support, not automated publishing or causal inference.

---

## 2. Data safety

### Dataset

This analysis uses the **FlyRank ML Internship dataset**.

The warehouse contains historical search and content-performance information used to investigate Search Intelligence questions.

The analysis was developed using a mid-panel historical period rather than relying on the final `_sample` month for feature development.

The final month is treated as a natural outcome/test window and was not used to develop the label logic.

### Development window

**Development month:** `[MONTH USED FOR DEVELOPMENT]`

**Date range:** `[START DATE]` to `[END DATE]`

The exact dates and tables used are recorded in the corresponding data-contract notebook.

### Tables used

The analysis uses:

- `[TABLE 1]`
- `[TABLE 2]`
- `[TABLE 3, IF APPLICABLE]`

Only the tables and columns necessary for the selected lane were used.

### Data deliberately excluded

The following fields or information were deliberately excluded from predictive features:

- `trend_direction`
- `trend_pct`
- future-window measurements
- target-derived variables
- post-outcome measurements
- client-identifying information
- URLs or private queries
- credentials or tokens
- pseudonymous identifiers as predictive features
- any field unavailable at the decision moment

### Label-derived leakage

Fields such as `trend_direction` and `trend_pct` were considered high-risk because they may encode information derived from the outcome being predicted.

They were therefore excluded from the final feature set.

A deliberate leakage experiment during the earlier development stage demonstrated why label-derived information must not be allowed into the final model.

The suspiciously strong performance observed under deliberate leakage was treated as evidence of leakage rather than evidence of a better model.

### Pseudonymous identifiers

Pseudonymous identifiers may be used for grouping or validation where appropriate.

They are not treated as predictive features because the model should learn from meaningful search signals rather than memorize individual entities.

### Public safety

No client-identifying details are intentionally included in the public report.

No private queries, credentials, tokens, or sensitive raw exports are included.

The public-facing work is limited to aggregate findings, methodology, model results, and decision-support recommendations.

---

## 3. Baseline

### Week-4 baseline

The first predictive approach was a transparent rule-based action score.

The rule uses selected historical search signals to assign:

1. a score
2. one reason code
3. an action label
4. a ranked queue

The baseline was intentionally kept simple so that its decisions could be inspected directly.

### Why the baseline is fair

The baseline provides a useful comparison because it represents a reasonable human-readable heuristic that could be implemented without machine learning.

The Week-5 model is evaluated using:

- the same task
- the same evaluation observations
- the same target
- the same split
- the same metric

This makes the comparison more meaningful than comparing unrelated metrics or datasets.

### Baseline metric

**Baseline metric:** `[BASELINE METRIC NAME]`

**Baseline score:** `[BASELINE SCORE]`

**Majority-class base rate:** `[BASE RATE]%`

The majority-class base rate is reported because a high accuracy or precision value can appear strong when one class is already common.

---

## 4. Model / analysis

### Method

The primary model is:

**`[MODEL NAME]`**

The model was selected because `[REASON THIS METHOD FITS THE LANE]`.

The goal was to test whether a relatively simple, interpretable model could provide useful signal beyond the Week-4 transparent baseline.

Complexity was not treated as a goal by itself.

### Final feature list

The final feature set contains:

1. `[FEATURE 1]`
2. `[FEATURE 2]`
3. `[FEATURE 3]`
4. `[FEATURE 4]`
5. `[FEATURE 5]`

Each feature was selected because it represents information available at the intended decision moment.

### Features deliberately excluded

The following were excluded:

- label-derived variables
- future-window variables
- post-outcome information
- pseudonymous IDs as predictive features
- client-identifying information
- private query information
- product flags where they would leak the outcome

### Target / proxy definition

**Target definition:**

> `[ONE-SENTENCE DESCRIPTION OF YOUR TARGET/LABEL]`

The target was constructed using the appropriate outcome window and was kept separate from the feature window.

### Assumptions

The analysis assumes that:

- historical signals are available before the decision moment
- the selected features are measured consistently
- the target represents a useful proxy for the prioritization question
- future information does not enter the feature set
- historical relationships may change over time

These assumptions limit how broadly the results should be interpreted.

---

## 5. Evaluation

### Validation design

A **time-aware split** was used.

Earlier observations were used for model development and later observations were used for validation.

This design is intended to better represent the real decision setting, where a recommendation would be made using information available before the outcome occurs.

**Training period:** `[TRAINING WINDOW]`

**Validation period:** `[VALIDATION WINDOW]`

### Why this split is honest

A random split can place observations from the same future period into both training and validation data.

A time-aware split better preserves the temporal direction of the problem.

It therefore provides a more realistic estimate of how the model behaves when applied to later observations.

### Model vs baseline

The model and baseline were evaluated on the same validation data using the same metric.

| Method | Metric | Score |
|---|---|---:|
| Week-4 baseline | `[METRIC]` | `[BASELINE SCORE]` |
| Week-5 model | `[METRIC]` | `[MODEL SCORE]` |

### Difference

**Model − baseline:** `[DIFFERENCE]`

The measured difference indicates that the model performed `[better / similarly / worse]` than the transparent baseline on this evaluation split.

This is an observed result on the evaluated data and does not establish guaranteed future performance.

### Base rate

**Majority-class base rate:** `[BASE RATE]%`

The base rate is included to provide context for classification metrics.

### Additional metrics

Where applicable:

| Metric | Baseline | Model |
|---|---:|---:|
| Accuracy | `[VALUE]` | `[VALUE]` |
| Precision | `[VALUE]` | `[VALUE]` |
| Recall | `[VALUE]` | `[VALUE]` |
| F1 | `[VALUE]` | `[VALUE]` |
| ROC-AUC | `[VALUE]` | `[VALUE]` |

Only metrics actually calculated by the notebook should be reported.

### Error analysis

The model's errors were observed primarily among `[DESCRIBE ERROR GROUP]`.

False positives included observations where `[DESCRIBE FALSE POSITIVE PATTERN]`.

False negatives included observations where `[DESCRIBE FALSE NEGATIVE PATTERN]`.

These errors suggest that the available signals do not fully capture the contextual factors involved in editorial decisions.

The model should therefore be used as a prioritization aid rather than as an automatic decision-maker.

---

## 6. Interpretation

### What the model found

The analysis found that the strongest measured signals were:

1. `[FEATURE 1]`
2. `[FEATURE 2]`
3. `[FEATURE 3]`

These signals were associated with the model's predictions within the evaluated dataset.

The findings are directional rather than causal.

### Feature importance

Feature importance or coefficient analysis indicates that:

- `[FEATURE 1]` contributed `[DESCRIPTION]`.
- `[FEATURE 2]` contributed `[DESCRIPTION]`.
- `[FEATURE 3]` contributed `[DESCRIPTION]`.
- `[FEATURE 4]` contributed `[DESCRIPTION]`.
- `[FEATURE 5]` contributed `[DESCRIPTION]`.

The interpretation describes model behavior rather than proving that changing a feature would cause a particular business outcome.

### Surprises

One observed result was:

> `[SURPRISING OR UNEXPECTED RESULT]`

This was useful because it showed `[WHAT YOU LEARNED]`.

### Negative results

A negative or weak result is also informative.

For example:

> `[NEGATIVE RESULT OR SIGNAL THAT DID NOT HELP]`

This indicates that the signal did not provide enough measured predictive value under the evaluated setup to justify relying on it strongly.

A lack of observed predictive value should not be interpreted as proof that the underlying concept can never matter.

### Honest interpretation

The model identifies patterns in the evaluated historical data.

It does not establish causality.

It does not prove how Google's ranking system works.

It does not guarantee that taking a recommended editorial action will improve future search performance.

---

## 7. Recommendation

### Ranked action framework

The final output is a ranked content-action queue.

Each observation receives:

- a priority score
- a rank
- one reason code
- an action recommendation

### Recommended actions

#### 1. High-priority review

Observations with stronger measured evidence should be reviewed first.

**Reason code:** `[REASON CODE]`

**Suggested action:** Investigate the page and its search context before making an editorial change.

---

#### 2. Content / metadata review

Observations showing a potential mismatch between visibility and engagement can be reviewed for metadata and content relevance.

**Reason code:** `[REASON CODE]`

**Suggested action:** Review title, description, search intent, content relevance, and competing results.

---

#### 3. Refresh review

Observations showing historical signs of declining or changing performance can be considered for refresh investigation.

**Reason code:** `[REASON CODE]`

**Suggested action:** Compare historical content and search intent before deciding whether a refresh is appropriate.

---

#### 4. Monitor

Observations with weaker or mixed evidence should be monitored rather than immediately changed.

**Reason code:** `[REASON CODE]`

**Suggested action:** Wait for additional evidence before taking a stronger action.

### How an editor could use the queue

A FlyRank editor could use the queue as a daily or periodic triage list.

The editor would:

1. Start with the highest-ranked observations.
2. Read the reason code.
3. Inspect the underlying search context.
4. Check whether the recommendation still makes sense.
5. Decide whether to act, monitor, or reject the recommendation.
6. Record useful feedback for future evaluation.

### Confidence

Confidence in an individual recommendation is limited by:

- model performance
- historical data quality
- signal availability
- distribution changes
- target quality
- contextual information not represented in the dataset

The queue should therefore be treated as **directional decision-support**.

### What should NOT be automated

The system should not automatically:

- publish content
- delete pages
- redirect pages
- rewrite important content
- make irreversible SEO changes
- claim that an action will increase traffic
- claim causal refresh impact
- treat the model score as a guaranteed outcome

Human review is required before action.

---

## 8. Reproducibility

All major analysis stages are stored in the repository.

### Repository

`[YOUR REPOSITORY URL]`

### Notebook sequence

```text
work/notebooks/
├── w03_data_contract.ipynb
├── w04_baseline_score.ipynb
├── w05_model.ipynb
├── w06_validation_audit.ipynb
├── w07_action_playbook.ipynb
└── capstone.ipynb
