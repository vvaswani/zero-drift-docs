# Zero Drift

This is an agentic sync system that watches a master source of truth, propagates changes across dependent API references and tooling, then regenerates documentation and creates PRs automatically.

See an example:

- Commit that modified the core API: https://github.com/vvaswani/zero-drift-docs/commit/efd9e55a5630d4c553430d1f4d7c7ab464854081
- Agentic update to documentation, gated on human review: https://github.com/vvaswani/zero-drift-docs/pull/12
- Agentic SDK regeneration, similarly gated: https://github.com/vvaswani/zero-drift-docs/pull/11

## What this demo shows

- a single source-of-truth API spec in `api/analytics-v1.yaml`
- downstream updates to SDKs and docs
- automated detection of spec drift
- regeneration of affected documentation
- PR-based update flow for docs and SDKs

## Repository layout

- `api/` - canonical OpenAPI source files
- `docs/how-to/` - user-facing guides that must stay aligned with the API
- `sdk/python/` - generated Python SDK and packaging metadata
- `scripts/docs_review_agent.py` - agent that checks spec diffs against docs
- `.github/workflows/` - automation for docs and SDK updates

## End-to-end flow

1. A change lands in the master API source under `api/`.
2. The docs review agent compares the spec diff against `docs/how-to/`.
3. If guidance has drifted, the affected docs are rewritten.
4. The Python SDK is regenerated from the updated OpenAPI spec.
5. Workflows open PRs with the refreshed docs and SDK code.

## Automation

### Docs updates

`.github/workflows/update-docs.yml` runs the docs review agent after API changes and opens a PR if the guides need updates.

### SDK regeneration

`.github/workflows/update-sdk.yml` regenerates the Python SDK from the OpenAPI spec and opens a PR when generated output changes.

## Local validation

### Review the API contract

Inspect `api/analytics-v1.yaml` to see the master contract that drives the rest of the system.

### Run the docs reviewer

```bash
python scripts/docs_review_agent.py
```

The script compares the current API spec diff to the `docs/how-to/` guides and updates them when needed.

### Regenerate the SDK

```bash
cd sdk/python
python scripts/generate_sdk.py
```

## Notes

- This repository demonstrates propagation of changes from a master source file through docs and SDKs, with automated PR creation for each.
