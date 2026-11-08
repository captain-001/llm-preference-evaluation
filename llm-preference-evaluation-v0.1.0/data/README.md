# Dataset

The experiment uses a course-provided Task 2 JSONL dataset described in
the report as a 100-example sample from Anthropic HH-RLHF.

The notebook downloads it automatically from:

`https://NLP-course-cuhksz.github.io/Assignments/Assignment1/task2/data/1.rlhf.jsonl`

Expected fields per JSONL record:

- `Question`
- `Answer1`
- `Answer2`
- `Preference` (`Answer1` or `Answer2`)

The raw dataset is intentionally **not included** in this repository.
Check the course rules and upstream dataset terms before copying or
redistributing it. The notebook downloads the file as `1.rlhf.jsonl` in
the current runtime directory.
