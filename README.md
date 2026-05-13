# ProofTable

**ProofTable is a manifest-first evidence model for high-stakes tables.**

It keeps a table readable for humans while selected cells resolve to structured claim objects that software, LLMs, and agent tools can inspect, validate, and reuse.

> A table cell is a claim. If the claim matters, it should carry evidence.

## What ProofTable does

A normal table cell might show:

```text
78.2%
```

A ProofTable-backed cell points to a claim object containing:

```text
value
unit
source
formula
inputs
caveat
freshness
use rule
proof hash
```

The table remains readable. The evidence becomes structured.

## Core model

```text
claim = value + unit + source + formula + inputs + caveat + freshness + use rule + hash
```

## Why it exists

Tables are often reused without the context behind their values.

Common failure modes:

- a percentage is copied without its denominator
- a formula-derived value is treated as directly observed
- a caveat is removed during summarization
- a stale source is treated as current
- a table is parsed by an LLM or agent and flattened into unsupported claims

ProofTable addresses this by attaching a machine-readable evidence object to selected table cells.

## Repository contents

```text
prooftable/
  README.md
  index.html
  package.json
  LICENSE
  CHANGELOG.md
  CONTRIBUTING.md
  spec/
    prooftable.schema.json
  src/
    validate.mjs
  tests/
    smoke-test.mjs
  examples/
    benchmark/
      prooftable.html
      prooftable.json
  docs/
    prooftable-model-presentation.html
  .github/
    workflows/
      prooftable.yml
```

## Quick start

Open the demo:

```text
index.html
```

Validate the example manifest:

```bash
npm install
npm run validate
npm test
```

Or run directly:

```bash
node src/validate.mjs examples/benchmark/prooftable.json
```

## Minimal HTML pattern

```html
<td data-pt-ref="claim-model-a-score">78.2%</td>
```

The cell points to a claim object in a manifest:

```json
{
  "id": "claim-model-a-score",
  "row": "Model A",
  "column": "Score",
  "display_value": "78.2%",
  "value": 78.2,
  "unit": "percent",
  "source_id": "benchmark-snapshot-demo",
  "formula": {
    "type": "percent",
    "expression": "passes / total * 100",
    "inputs": {
      "passes": 176,
      "total": 225
    },
    "precision": 1
  },
  "quality": {
    "status": "caution",
    "reason": "Related denominator field differs."
  },
  "use_rule": {
    "may_cite": true,
    "must_preserve_caveat": true
  },
  "proof_hash": "sha256:..."
}
```

## Validation severity model

| Status | Meaning |
|---|---|
| `pass` / `clean` | Claim verifies without required caveat |
| `caution` | Claim is usable only if caveat is preserved |
| `warn` | Non-blocking issue |
| `fail` | Claim should not be reused |
| `unknown` | Insufficient evidence |

## What validation checks

The included validator checks:

- manifest parses
- sources exist
- claim IDs are unique
- each claim has required fields
- `source_id` resolves
- supported formulas recompute
- proof hashes match canonical claim objects
- caveated claims include caveat-preserving use rules

## Trust boundary

ProofTable verifies traceability and consistency.

It does **not** prove the underlying source is true.

It verifies that a table claim is:

- source-attached
- formula-checkable
- caveat-preserving
- freshness-aware
- hash-verifiable

## Best use cases

ProofTable is appropriate for tables where a cell affects interpretation, reuse, citation, or audit.

Good use cases:

- AI benchmark reports
- clinical evidence tables
- scientific summaries
- financial assumption tables
- compliance matrices
- engineering test reports

Avoid using ProofTable for ordinary layout tables, simple content grids, or low-stakes tables where source/formula/caveat/freshness do not materially affect interpretation.

## Product direction

The intended product form is a small open-source validator/compiler:

```bash
prooftable validate report.html
prooftable audit benchmark.json
prooftable build results.csv
prooftable diff old.json new.json
```

## Author

By Jimi Plouffe  
james@seekmomentum.com  
Momentum, LLC
