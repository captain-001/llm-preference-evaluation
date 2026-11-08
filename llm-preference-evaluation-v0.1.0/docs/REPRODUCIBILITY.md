# Reproducibility Notes

## Environment

The canonical experiment notebook targets Google Colab. The current code
reads the API key through `google.colab.userdata`, so it is not yet a
portable local Python application.

The dependency list in `requirements.txt` records the main imported
libraries. Exact package versions were not pinned in the original
experiment; therefore this snapshot does not claim bit-for-bit
environment reproducibility.

## API credentials

1.  Open Google Colab’s Secrets panel.
2.  Add a secret named `DEEPSEEK_API_KEY`.
3.  Grant the notebook access to that secret.
4.  Never write the key into a code cell or commit it to Git.

## Dataset

The notebook downloads the 100-record JSONL dataset from the course URL
embedded in the notebook. The data file is not committed to this
repository. See `data/README.md`.

## Seeds and split

- Answer-order randomization seed: `2026`.
- Few-shot selection seed: `42`.
- Four few-shot examples: IDs `7`, `13`, `84`, `99`.
- Evaluation size: `96`.
- Evaluation answer order: 49 original, 47 swapped.

## Re-running

Run `notebooks/01_preference_evaluation.ipynb` from top to bottom. It
makes paid API calls. The final cell exports CSV tables and experiment
metadata to `task2_results/` and creates `task2_results.zip` in the
notebook runtime.

For a meaningful comparison with a future version, keep the dataset
version, sample IDs, answer-order assignment, model identifier, prompt
text and generation settings fixed. Record any changes in a new
experiment configuration rather than silently overwriting the original
snapshot.
