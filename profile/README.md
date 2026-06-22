# Open Delivery Spec

> **Zero-config AI code quality gate for teams using Claude Code, GitHub Copilot, or Cursor.**
> These tools already write `Co-Authored-By` trailers to every commit. ODS reads them
> automatically in CI — detecting AI-generated code, analyzing quality, scoring technical
> debt, and enforcing policy on every PR.

## How it works

ODS runs as a four-step pipeline on every pull request:

1. **Detect** — reads `Co-Authored-By` commit trailers (primary signal), ODS trailer fields,
   PR body, branch name, and diff heuristics to determine AI involvement with a confidence score
2. **Analyze** — identifies AI-specific quality defects: redundant error handling,
   over-commenting, missing edge cases, unsafe deserialization, inconsistent patterns
3. **Score** — calculates technical debt impact across five weighted dimensions
4. **Check** — evaluates your OPA Rego policy; blocks, warns, or passes the PR

Signals are heuristic — ODS is a **signal producer, not a quality oracle**. A
`PASS` means no deny rule fired. A detection at 85% confidence means ODS is 85% confident
the code is AI-assisted, not that exactly 85% of lines were written by an AI.

## Quick Start

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

Or scaffold everything locally:

```bash
go install github.com/open-delivery-spec/cli/cmd/ods@latest
ods init        # creates workflow, .ods/policy.rego, AGENTS.md, Cursor rules
ods hook install
```

## Repositories

| Repo | What it is |
|------|-----------|
| [spec](https://github.com/open-delivery-spec/spec) | Core specification, design philosophy, case studies |
| [cli](https://github.com/open-delivery-spec/cli) | Go CLI — `ods detect / analyze / score / check / init` |
| [validate-action](https://github.com/open-delivery-spec/validate-action) | GitHub Action wrapping the full pipeline |

## Positioning

Unlike **OpenSSF Scorecard** (which measures supply-chain security practices) and
**SLSA** (which tracks artifact provenance), ODS focuses on the gap neither covers:
whether AI-generated code is safe to merge — and whether your team can prove it.

Licensed under Apache 2.0.
