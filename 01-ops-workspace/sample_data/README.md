# Sample Data Contract

This directory contains the Phase 0 seed samples delivered by Ops for downstream
pipeline bring-up.

## Files

- `samples_v1.jsonl`: 15 desensitized text+image sample records covering the 5
  first-level labels in `docs/label-taxonomy.md`
- `sample_images/`: repo-local PNG fixtures referenced by `samples_v1.jsonl`

## Important Limitation

The images under `sample_images/` are lightweight local fixtures for:

- path existence checks
- multimodal loader smoke tests
- basic end-to-end JSONL ingestion validation

They are **not** semantically meaningful business samples and must **not** be
used for:

- image-conditioned reasoning quality evaluation
- multimodal model effect benchmarking
- final prompt or policy quality acceptance

If Dev/Algo needs image-semantic validation, please request a separate
desensitized image set outside git and document that delivery independently.
