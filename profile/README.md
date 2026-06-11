# 🚀 Open Delivery Spec

> **Detect AI-generated code, analyze its quality, and prevent technical debt — before it reaches production.**

AI writes code faster than ever. But AI code increases technical debt in predictable ways — hallucinated APIs, redundant error handling, over-commenting, missing tests, and invisible AI authorship. ODS is the CI gate that stops this.

## The Four Steps

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

## Repositories

| Repository | Description |
|-----------|-------------|
| [spec](https://github.com/open-delivery-spec/spec) | Core specification, design principles, roadmap |
| [cli](https://github.com/open-delivery-spec/cli) | CLI — `ods detect | analyze | score | check | hook | init` |
| [validate-action](https://github.com/open-delivery-spec/validate-action) | GitHub Action for CI compliance |

## Quick Start

```bash
# Install
go install github.com/open-delivery-spec/cli/cmd/ods@latest

# Detect AI code in your PR
ods detect

# Analyze AI code quality
ods analyze

# Score technical debt impact
ods score

# Enforce enterprise policy
ods check

# Install pre-commit hooks
ods hook install

# Scaffold CI workflow and agent instructions
ods init
```

## Detection Signals

ODS detects AI code without relying on developer self-disclosure:

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
| `ai-over-commenting` | Comment-to-code ratio >40% | medium-high |
| `ai-missing-edge-case` | if-statements without else branches | low |
| `ai-unsafe-deserialization` | Unsafe type assertions / unmarshals | high |
| `ai-inconsistent-pattern` | Mixed naming / indentation styles | medium-low |

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
```

## CI Integration

```yaml
- uses: open-delivery-spec/validate-action@v1
  with:
    check: all
```

## Design Principles

1. **Detect, don't rely on disclosure.** Multiple independent signal sources for AI detection.
2. **Deterministic rules, probabilistic signals.** Quality rules are yes/no. Detection confidence is a signal, not a verdict.
3. **Tool-agnostic.** Works with any CI/CD that can run a binary.
4. **Policy as code.** Enterprise rules in Rego, version-controlled alongside code.
5. **Prevent, don't just report.** Pre-commit hooks and CI gates block problems before merge.

## License

Apache 2.0
