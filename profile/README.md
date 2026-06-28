<div align="center">

# Open Delivery Spec

### Zero-config AI code quality gate for every pull request

**Claude Code, GitHub Copilot, and Cursor already stamp `Co-Authored-By` trailers on every commit.**
ODS reads them automatically in CI — attributing AI-generated code, analyzing its quality,
scoring technical-debt impact, and enforcing your policy before merge.

[![Spec](https://img.shields.io/badge/spec-read-blue?logo=readthedocs&logoColor=white)](https://github.com/open-delivery-spec/spec)
[![CLI](https://img.shields.io/badge/CLI-Go-00ADD8?logo=go)](https://github.com/open-delivery-spec/cli)
[![GitHub Action](https://img.shields.io/badge/GitHub_Action-v1-2088FF?logo=githubactions&logoColor=white)](https://github.com/open-delivery-spec/validate-action)
[![License](https://img.shields.io/badge/license-Apache_2.0-green?logo=apache)](https://github.com/open-delivery-spec/spec/blob/main/LICENSE)

</div>

---

## The pipeline

ODS runs four steps on every pull request:

```
   PR opened
      │
      ▼
 ①  Detect   →  Is there AI code?            (Co-Authored-By trailers, branch prefix, PR disclosure, diff heuristics)
      │
      ▼
 ②  Analyze  →  What quality defects?        (built-in AI heuristics + imported SARIF findings)
      │
      ▼
 ③  Score    →  How much tech debt added?    (quality-driven, weighted by AI risk)
      │
      ▼
 ④  Check    →  Block, warn, or pass?        (your OPA Rego policy)
      │
      ▼
  PASS · WARN · BLOCK   +   PR comment · job summary · HTML report · badge
```

Signals are heuristic — ODS is a **signal producer, not a quality oracle**. A `PASS` means no
deny rule fired; an 85% detection means ODS is 85% confident the code is AI-assisted, not that
85% of the lines were.

## Quick start

Add the Action — that's the whole setup:

```yaml
# .github/workflows/ods.yml
name: ODS AI Code Quality
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  ods:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/checkout@v7
        with:
          fetch-depth: 0
      - uses: open-delivery-spec/validate-action@v1
        with:
          diff-base: ${{ github.event.pull_request.base.sha }}
          pr-body: ${{ github.event.pull_request.body }}
          branch: ${{ github.head_ref }}
```

Prefer the CLI, or want it locally too?

```bash
go install github.com/open-delivery-spec/cli/cmd/ods@latest
ods init           # scaffolds .github/workflows/ods-ai-quality.yml + .ods/policy.rego
ods hook install   # optional: block low-quality AI code before it leaves your machine
```

## Repositories

| Repo | What it is |
|------|------------|
| 📘 [**spec**](https://github.com/open-delivery-spec/spec) | The specification, design philosophy, docs site, and case studies |
| ⚙️ [**cli**](https://github.com/open-delivery-spec/cli) | Go CLI — `ods detect · analyze · score · check · init · hook` |
| 🤖 [**validate-action**](https://github.com/open-delivery-spec/validate-action) | One-step GitHub Action wrapping the full pipeline |

## Where ODS fits

Unlike **OpenSSF Scorecard** (supply-chain security practices) and **SLSA** (artifact provenance),
ODS targets the gap neither covers: **whether AI-generated code is safe to merge — and whether your
team can prove it.** It's tool-agnostic, policy-driven, and machine-readable by design.

<div align="center">

Apache 2.0 · Built in the open

</div>
