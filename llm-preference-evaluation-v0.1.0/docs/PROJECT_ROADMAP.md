# Project Roadmap

The project is planned in five phases. Phase 1 captures the current
assignment-based experiment; later phases add research depth and
software engineering maturity.

## Phase 1 — Reproducible baseline (current: v0.1.0)

- Preserve the five current methods: zero-shot, few-shot, CoT,
  self-consistency and multi-agent.
- Keep a common 96-example held-out evaluation set.
- Preserve answer-order randomization and label remapping.
- Record accuracy, validity, latency and token usage.
- Retain statistical tests, Pareto analysis, ablation, bias analysis and
  case studies.
- Organize the notebook, experiment configuration, results summary and
  report.

**Exit goal:** a clearly documented and repeatable baseline experiment.

## Phase 2 — Judge reliability and bias

- Expand answer-order robustness testing with repeated paired swaps and
  multiple seeds.
- Investigate verbosity bias, position bias and self-preference bias.
- Add confidence and calibration analysis.
- Compare AI-judge decisions with independent human annotations where
  feasible.
- Add disagreement and error taxonomies.

**Exit goal:** characterize when and why an LLM judge makes unreliable
preference decisions.

## Phase 3 — Preference learning

- Explore pairwise preference modeling, including Bradley–Terry-style
  analysis.
- Train or evaluate a reward model using preference pairs.
- Study reward-model generalization and calibration.
- Explore Direct Preference Optimization (DPO) as a later extension.

**Exit goal:** extend from judging preference pairs to learning
preference signals.

## Phase 4 — Advanced multi-agent evaluation

- Test role design and agent independence.
- Require evidence-grounded rationales for evaluator decisions.
- Improve Coordinator routing and disagreement handling.
- Measure the quality/cost trade-off of different agent topologies.
- Add confidence-based routing and agent ablations.

**Exit goal:** determine whether multi-agent designs provide measurable
benefits over strong single-judge baselines.

## Phase 5 — Open-source release

- Refactor notebook logic into reusable modules under `src/`.
- Add a stable evaluator interface and configuration-driven experiment
  runner.
- Add unit/integration tests and continuous integration.
- Add command-line usage, documentation and reproducibility
  instructions.
- Add result visualization and export interfaces.
- Select and add an explicit open-source license after confirming
  ownership and dataset terms.

**Exit goal:** a documented, tested and reusable open-source evaluation
toolkit.
