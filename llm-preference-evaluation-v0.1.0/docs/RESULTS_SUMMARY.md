# Results Summary

This document records the results reported by the current notebook and
accompanying experiment report. It is a single-run snapshot on the
common 96-example test set.

## Method comparison

| Method             | Accuracy | Valid output | Avg. latency / pair | Avg. tokens / pair | Macro-F1 |
|--------------------|---------:|-------------:|--------------------:|-------------------:|---------:|
| Zero-shot baseline |   61.46% |      100.00% |             0.989 s |                246 |   59.89% |
| Few-shot           |   61.46% |      100.00% |             1.019 s |              1,073 |   61.12% |
| Chain-of-Thought   |   66.67% |       98.96% |             2.545 s |                506 |   66.09% |
| Self-consistency   |   63.54% |      100.00% |             8.103 s |              1,545 |   62.86% |
| Multi-agent        |   61.46% |      100.00% |             5.337 s |              1,143 |   61.12% |

CoT had the highest observed accuracy (64/96) and macro-F1. The
zero-shot baseline had the lowest average latency and token usage. The
multi-agent system produced valid decisions for all cases, but its
accuracy matched the baseline despite higher cost.

## Statistical tests

- Cochran’s Q for overall accuracy across five methods: `Q = 1.92`,
  `p = 0.7505` (not significant).
- CoT vs. baseline: observed accuracy difference `+5.21` percentage
  points; raw paired p-value `0.3323`; Holm-adjusted p-value `1.000`;
  reported 95% bootstrap CI `[-3.13, 13.54]` percentage points.
- Friedman test for latency: `χ² = 364.14`, `p < 0.001`, Kendall’s
  `W = 0.948`.
- Friedman test for total tokens: `χ² = 338.13`, `p < 0.001`, Kendall’s
  `W = 0.881`.

The accuracy tests did not establish a statistically significant
difference. The cost differences were statistically clearer in this run.

## Answer-order analysis

The held-out test set contained 49 original-order and 47 swapped-order
examples. None of the five Fisher exact tests remained significant after
Holm correction; all adjusted p-values were `1.000`.

| Method           | Original accuracy | Swapped accuracy | Difference (swapped - original) |  Raw p |
|------------------|------------------:|-----------------:|--------------------------------:|-------:|
| Baseline         |            57.14% |           65.96% |                        +8.81 pp | 0.4078 |
| Few-shot         |            61.22% |           61.70% |                        +0.48 pp | 1.0000 |
| CoT              |            65.31% |           68.09% |                        +2.78 pp | 0.8305 |
| Self-consistency |            65.31% |           61.70% |                        -3.60 pp | 0.8325 |
| Multi-agent      |            55.10% |           68.09% |                       +12.98 pp | 0.2139 |

These are descriptive differences; the experiment does not establish a
statistically reliable answer-order effect.

## Multi-agent ablation

| Configuration                        | Accuracy | Correct samples | Coordinator escalation |
|--------------------------------------|---------:|----------------:|-----------------------:|
| Full multi-agent                     |   61.46% |              59 |                 28.13% |
| Without Accuracy Evaluator           |   63.54% |              61 |                 17.71% |
| Without Helpfulness Evaluator        |   62.50% |              60 |                 13.54% |
| Without Quality and Safety Evaluator |   65.63% |              63 |                 25.00% |

The reduced configurations scored higher descriptively in this run. No
significance test was reported for these ablation differences, so they
should not be interpreted as proof that a particular agent role is
harmful.

## Limitations

- Only 96 held-out pairs were evaluated.
- The experiment used one model/API family and one parameter
  configuration.
- Latency depends on API and network conditions.
- Human preference labels can be ambiguous.
- Preference-label agreement is not equivalent to factual correctness.
- A larger dataset, repeated splits, multiple models and independent
  human annotators are needed for stronger conclusions.
