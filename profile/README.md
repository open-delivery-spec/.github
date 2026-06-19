# 🚀 Open Delivery Spec

> **AI writes the code. ODS governs the delivery.**

## The problem it solves

A PR arrives: 8 commits, 6 written by Copilot, 2 by a human. The branch says `feature/add-sarif-output`. But two changed files touch the authentication module — nothing to do with SARIF. The reviewer doesn't know. The merge happens. A bug ships.

**ODS catches this before the merge.** In CI. Without requiring any reviewer to be an expert.

ODS is the CI gate that detects AI-generated code, analyzes its quality, scores technical debt impact, and enforces enterprise policy — on every pull request.

```
PR arrived
   │
   ▼
① Detect  — Which code is AI-generated? (multi-source, no self-disclosure required)
   │
   ▼
② Analyze — What quality defects does the AI code have? (5 rule categories)
   │
   ▼
③ Score   — How much technical debt does this PR add? (5-dimension weighted score)
   │
   ▼
④ Enforce — Should this PR be blocked? (OPA Rego policies)
   │
   ▼
PASS / WARN / BLOCK
```

## Quick Start

```bash
# Install
go install github.com/open-delivery-spec/cli/cmd/ods@latest

# Detect AI code in your PR
ods detect --diff-base origin/main --branch feature/my-feature

# Analyze AI code quality
ods analyze --json

# Score technical debt impact
ods score --json

# Enforce enterprise policy
ods check
```

Or use the one-step GitHub Action on every PR:

```yaml
- uses: actions/checkout@v7
  with:
    fetch-depth: 0
- uses: open-delivery-spec/validate-action@v1
```

## Repositories

| Repository | Description |
|-----------|-------------|
| [spec](https://github.com/open-delivery-spec/spec) | Core specification, design principles, roadmap |
| [cli](https://github.com/open-delivery-spec/cli) | CLI — `ods detect | analyze | score | check | hook | init` |
| [validate-action](https://github.com/open-delivery-spec/validate-action) | GitHub Action — AI code quality gate for CI |

## What ODS Detects

| Signal | Source | Confidence |
|---|---|---|
| Git commit trailers | `AI-assisted: true`, `AI-tool: name` | 90% |
| PR body AI disclosure | Checkbox and section parsing | 85% |
| Branch name prefix | `ai-*` convention | 35–50% |
| Diff heuristics | Comment ratio, verbose naming, error patterns | 40% |

## Analysis Rules

| Rule | What it detects | Severity |
|---|---|---|
| `ai-redundant-error-handling` | Dense clusters of if-err-nil blocks | medium |
| `ai-over-commenting` | Comment-to-code ratio >40% (≥50% → high) | medium / high |
| `ai-missing-edge-case` | if-statements without else branches | low |
| `ai-unsafe-deserialization` | json.Unmarshal into interface{} | high |
| `ai-inconsistent-pattern` | Mixed naming / indentation styles | medium / low |

## How ODS Relates to APP and C2PA

**APP** (AI Content Provenance) and **C2PA** answer the question: *"Was this text/image AI-generated?"* — targeting content platforms and EU AI Act compliance. **ODS** answers a different question: *"Is this AI-generated code safe and ready to ship?"* — targeting software engineering teams and CI/CD pipelines. They are complementary: you might use C2PA to mark AI-generated documentation, and ODS to govern how that documentation's PR was reviewed and deployed.

## Used By

| Project | Modules Used | Since |
|---|---|---|
| [open-delivery-spec/spec](https://github.com/open-delivery-spec/spec) | L1 validation | June 2026 |
| [open-delivery-spec/cli](https://github.com/open-delivery-spec/cli) | L1 validation | June 2026 |
| [open-delivery-spec/validate-action](https://github.com/open-delivery-spec/validate-action) | L1 validation | June 2026 |

## Enterprise Policy

Define OPA Rego policies in `.ods/policy.rego`:

```rego
package ods.policy

default allow := true

deny[msg] {
    input.ai_confidence > 0.8
    input.test_coverage < 0.3
    msg = "AI code with low test coverage"
}

warn[msg] {
    input.ai_generated == true
    input.ai_confidence > 0.6
    count(input.issues) > 2
    msg = "High-confidence AI code with multiple quality issues"
}
```

## Design Principles

1. **Detect, don't rely on disclosure.** Multiple independent signal sources for AI detection.
2. **Deterministic rules, probabilistic signals.** Quality rules are yes/no. Detection confidence is a signal, not a verdict.
3. **Tool-agnostic.** Works with any CI/CD that can run a binary.
4. **Policy as code.** Enterprise rules in Rego, version-controlled alongside code.
5. **Prevent, don't just report.** Pre-commit hooks and CI gates block problems before merge.

## License

Apache 2.0
