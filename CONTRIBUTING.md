# Contributing

ProofTable is currently an early reference implementation.

Good first contributions:

- add real-world examples
- improve `spec/prooftable.schema.json`
- add additional formula validators
- add CSV/Markdown input adapters
- add better diff output
- test the validator against real benchmark, scientific, compliance, or engineering tables

Design constraints:

- keep the visible table readable
- keep the manifest canonical
- do not execute arbitrary formula strings
- preserve caveats as first-class claim data
- distinguish validation failures from evidence cautions
- do not claim ProofTable proves source truth
