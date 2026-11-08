# LLM Preference Evaluation

**A research-oriented framework for evaluating LLM-as-a-Judge preference
prediction.**

Version: `v0.1.0` · Status: notebook-based research prototype

This project studies how prompting strategies and role-based agents
affect an LLM judge’s ability to predict human preferences between two
candidate answers. It began as a course assignment and is being
organized into a reproducible, extensible research project.

> **Scope of this release:** this is an experiment-first repository. The
> main implementation is a Google Colab notebook; a reusable Python
> package and command-line interface have not yet been implemented.

## What is included

- Five evaluation strategies: zero-shot baseline, few-shot prompting,
  chain-of-thought (CoT), self-consistency, and role-based multi-agent
  evaluation.
- Answer-order randomization with label remapping.
- A shared 96-example held-out evaluation set.
- Paired error analysis and statistical tests.
- Latency and token-cost comparisons, Pareto analysis, and multi-agent
  ablation.
- Answer-order bias analysis, classification metrics, representative
  cases, and visualizations.
- Export of per-method CSV files and experiment metadata when the
  notebook is run.

## Current experimental snapshot

The experiment used `deepseek-chat` through `langchain-deepseek`. The
course-provided dataset contains 100 answer pairs. Four balanced
few-shot examples were held out from evaluation, leaving 96 examples
shared by all five methods.

| Method             | Accuracy | Valid output | Avg. latency / pair | Avg. tokens / pair | Macro-F1 |
|--------------------|---------:|-------------:|--------------------:|-------------------:|---------:|
| Zero-shot baseline |   61.46% |      100.00% |             0.989 s |                246 |   59.89% |
| Few-shot           |   61.46% |      100.00% |             1.019 s |              1,073 |   61.12% |
| Chain-of-Thought   |   66.67% |       98.96% |             2.545 s |                506 |   66.09% |
| Self-consistency   |   63.54% |      100.00% |             8.103 s |              1,545 |   62.86% |
| Multi-agent        |   61.46% |      100.00% |             5.337 s |              1,143 |   61.12% |

These are the recorded results from this run, not a claim that one
method is universally superior. The overall accuracy difference was not
statistically significant (Cochran’s Q = 1.92, p = 0.7505). The observed
CoT improvement over the baseline was 5.21 percentage points, but its
paired comparison was not significant after correction. See
[`docs/RESULTS_SUMMARY.md`](docs/RESULTS_SUMMARY.md) for details and
limitations.

## Repository structure

``` text
llm-preference-evaluation-v0.1.0/
├── README.md
├── README_zh.md
├── CHANGELOG.md
├── requirements.txt
├── .gitignore
├── configs/
│   └── experiment_config.json
├── data/
│   └── README.md
├── docs/
│   ├── EXPERIMENT_DESIGN.md
│   ├── PROJECT_ROADMAP.md
│   ├── REPRODUCIBILITY.md
│   └── RESULTS_SUMMARY.md
├── notebooks/
│   ├── 01_preference_evaluation.ipynb
│   └── archive/
│       ├── 00_original_coursework.ipynb
│       └── 01_randomized_fixed_intermediate.ipynb
├── reports/
│   ├── README.md
│   └── preference_evaluation_report.tex
├── results/
│   ├── README.md
│   ├── method_comparison.csv
│   ├── overall_statistical_tests.csv
│   ├── answer_order_bias.csv
│   └── multi_agent_ablation.csv
└── tests/
    └── README.md
```

## Quick start

The canonical notebook is designed for **Google Colab**.

1.  Upload or open `notebooks/01_preference_evaluation.ipynb` in Google
    Colab.
2.  Add a Colab Secret named `DEEPSEEK_API_KEY`.
3.  Run the notebook from top to bottom.
4.  The notebook downloads the course dataset from the source URL
    recorded in the notebook, validates the 100 records, and creates the
    randomized split.
5.  At the end, the notebook exports detailed tables and experiment
    settings to `task2_results/` and creates `task2_results.zip` in the
    Colab runtime.

Install the Python dependencies if you are running in a compatible
notebook environment:

``` bash
pip install -r requirements.txt
```

The notebook currently imports `google.colab.userdata`; local execution
outside Colab therefore requires a future configuration refactor. Do not
paste API keys into notebook cells or commit them to Git.

## Experimental design

- Model: `deepseek-chat`
- Baseline / few-shot / CoT / agent / coordinator temperature: `0`
- Self-consistency temperature: `0.7`
- Self-consistency samples per example: `5`
- Answer-order randomization seed: `2026`
- Few-shot selection seed: `42`
- Few-shot examples: `4` (two from each displayed preference class)
- Held-out evaluation set: `96` examples
- Test-set answer order: 49 original-order pairs and 47 swapped-order
  pairs

The four few-shot examples are excluded from the common evaluation set.
Predictions are mapped back to the original answer identities before
scoring.

## Important notes

- **Dataset is not bundled.** The notebook downloads the course-provided
  JSONL file from the URL in `data/README.md`. Check the dataset’s terms
  and course rules before redistributing it.
- **API costs apply.** Running all methods makes multiple DeepSeek API
  calls, especially self-consistency and multi-agent evaluation.
- **Results are a recorded snapshot.** API model versions, service
  conditions, and latency may change between runs.
- **No license has been selected in this initial snapshot.** A public
  GitHub repository is not automatically open source. Choose and add a
  license before inviting reuse or contributions.
- The course submission PDF containing a student identifier is
  intentionally not included. The report source in `reports/` has been
  anonymized.

## Roadmap

See [`docs/PROJECT_ROADMAP.md`](docs/PROJECT_ROADMAP.md) for the
five-stage development plan, from the current assignment-based
experiment to a modular open-source evaluation toolkit.

## Citation and attribution

The experiment uses a course-provided Task 2 dataset sampled from
Anthropic HH-RLHF, as described in the accompanying report. The dataset
itself is not redistributed in this repository. Please retain the
original dataset attribution and follow the dataset and course terms
when using it.

## Current limitations

- One 100-example dataset and one 96-example held-out split.
- One model/API family and one configuration.
- The current implementation is notebook-centric rather than a reusable
  package.
- Accuracy agreement with preference labels is not the same as factual
  correctness.
- The observed accuracy ranking is descriptive; statistical testing did
  not establish a significant overall difference.
