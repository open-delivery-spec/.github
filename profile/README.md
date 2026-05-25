# 🚀 Open Delivery Spec

**An open specification for machine-readable delivery governance evidence in the AI era.**

AI writes code faster than ever — [90% of developers use AI daily](https://dora.dev/research/2025/). But delivery governance hasn't kept up. Open Delivery Spec defines standardized, machine-parseable schemas for core software delivery governance artifacts.

## What We Solve

| Before Merge | At Merge | After Merge |
|---|---|---|
| Branch naming | PR descriptions | CI failure analysis |
| Commit messages | AI change review | Release readiness |
| | Approval workflow | Rollback plans |
| | | Production audit evidence |

## Repositories

| Repository | Description |
|-----------|-------------|
| [spec](https://github.com/open-delivery-spec/spec) | Core specification — 9 modules with JSON Schemas (includes documentation) |
| [cli](https://github.com/open-delivery-spec/cli) | Reference CLI tool for validation and generation |
| [validate-action](https://github.com/open-delivery-spec/validate-action) | GitHub Action for automated compliance checks |

> [!NOTE]
> **Current maturity**: Modules 01–03 (Branch Naming, Commit Message, PR Description) are **Candidate**. Modules 04–09 are **Draft**. See [ROADMAP.md](https://github.com/open-delivery-spec/spec/blob/main/ROADMAP.md).

## Spec Modules

1. **Branch Naming** — Standardized branch names (extends Conventional Branch)
2. **Commit Message** — AI-attributable commit format (extends Conventional Commits)
3. **PR Description** — Structured PR body with mandatory AI disclosure
4. **AI Change Review** — Three-level review protocol (L1/L2/L3)
5. **CI Failure** — Machine-parseable failure reports with AI explanation
6. **Release Readiness** — Evidence-based release gates with scoring
7. **Approval Workflow** — Declarative, AI-aware approval policies
8. **Rollback Plan** — Minimum requirements for valid rollback plans
9. **Production Release Evidence** — Immutable, auditable deployment bundles

## Quick Start

```bash
# Install CLI
go install github.com/open-delivery-spec/cli/cmd/ods@latest

# Validate branch naming
ods validate branch feature/add-oauth-login

# Use GitHub Action (start with branch naming — the simplest check)
- uses: open-delivery-spec/validate-action@v1
  with:
    check: branch-naming
    branch_name: ${{ github.head_ref }}
```

## Design Principles

- **Machine-first, human-readable** — Every artifact has a JSON Schema
- **AI-native** — AI agents are first-class participants
- **Composable** — Use one module or all nine
- **Tool-agnostic** — Works with any CI/CD, AI tool, or VCS
- **Audit-ready** — Every artifact carries evidence for compliance

## License

All repositories are [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) licensed.
