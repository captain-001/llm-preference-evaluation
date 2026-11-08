# Tests

The current release is notebook-first and does not yet contain a modular
Python implementation or automated test suite.

The notebook performs data validation, checks the expected dataset size
and labels, checks the held-out split size, and validates one-to-one
joins when assembling predictions. A dedicated `tests/` suite will be
added during the modularization phase.
