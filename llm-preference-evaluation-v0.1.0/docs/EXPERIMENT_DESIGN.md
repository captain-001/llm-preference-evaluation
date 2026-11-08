# Experiment Design

## Research question

How do different prompting and agent-based strategies affect an LLM
judge’s ability to predict the human-preferred answer in a pairwise
comparison, and what are their latency and token costs?

## Dataset and split

- Source: course-provided Task 2 JSONL dataset, described as a
  100-example sample from Anthropic HH-RLHF.
- Fields: `Question`, `Answer1`, `Answer2`, `Preference`.
- Original label distribution: 55 `Answer1`, 45 `Answer2`.
- Answer order: 50 of the 100 pairs are swapped using seed `2026`; the
  preference label is swapped at the same time.
- Few-shot examples: four examples, balanced two per displayed
  preference class, selected with seed `42`.
- Few-shot IDs reported by the experiment: 7, 13, 84, and 99.
- Common evaluation set: the remaining 96 sample IDs.
- Evaluation answer order: 49 original-order and 47 swapped-order pairs.

All method predictions are mapped back to the original answer identities
before scoring. The four few-shot examples are excluded from the common
test set.

## Model and API configuration

- Provider/model: DeepSeek `deepseek-chat`.
- Client: `langchain-deepseek` / LangChain.
- Baseline, few-shot, CoT, agent and coordinator temperature: `0`.
- Self-consistency temperature: `0.7`.
- Self-consistency votes: `5`.
- API key: read from Google Colab Secrets under `DEEPSEEK_API_KEY`.

## Methods

1.  **Zero-shot baseline** — a direct pairwise comparison prompt asking
    for one preference label.
2.  **Few-shot** — the baseline task augmented with four balanced
    labeled demonstrations.
3.  **Chain-of-Thought (CoT)** — a structured comparison prompt that
    asks the judge to consider the user’s request, relevance,
    correctness, clarity, helpfulness and safety before producing a
    label.
4.  **Self-consistency** — five independently sampled judgments at
    temperature `0.7`; the majority label is selected.
5.  **Multi-agent** — three role-specific evaluators (accuracy,
    helpfulness, and quality/safety). Unanimous decisions are accepted;
    disagreements or invalid outputs are escalated to a Coordinator.

## Metrics and analysis

- Preference-label accuracy.
- Valid prediction rate.
- Macro precision, macro recall, macro-F1 and weighted F1.
- Average wall-clock latency per pair.
- Average input/output/total tokens per pair.
- Paired error analysis and paired significance tests.
- Pareto analysis across accuracy, latency and token usage.
- Leave-one-agent-out ablation.
- Accuracy by answer order and Fisher exact tests.
- Representative consensus and disagreement cases.

## Interpretation guardrails

Accuracy measures agreement with the dataset’s preference label, not
factual correctness. The experiment uses one small held-out split and
one model/API family. Results should therefore be treated as a
preliminary snapshot rather than a general ranking of methods.
