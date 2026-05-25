# 🚀 Open Delivery Spec

**A lightweight, machine-readable standard for AI-aware pull request and delivery metadata.**

> ODS does not prove the code is correct. It proves the delivery process contains the minimum structured evidence needed for humans and machines to review the change responsibly.

AI writes code faster than ever. But delivery governance — review, verification, audit — hasn't kept up. ODS defines a small set of machine-readable metadata conventions that CI tools and AI agents can validate before merge.

## The Wedge: ODS L1 + AI Disclosure

Three checks that run in CI. Adopt in ~5 minutes.

| Before ODS | After ODS L1 + AI Disclosure |
|---|---|
| `fix stuff` — empty PR body | `fix(auth): handle expired OAuth state` — Summary, AI Disclosure, Changes, Testing |
| Reviewer asks "what does this do? which part is AI?" | Reviewer has answers before opening the PR |

```yaml
- uses: open-delivery-spec/validate-action@v1
  with:
    check: all
    branch_name: ${{ github.head_ref }}
    pr_body: ${{ github.event.pull_request.body }}
```

## Repositories

| Repository | Description |
|-----------|-------------|
| [spec](https://github.com/open-delivery-spec/spec) | Core specification, JSON Schemas, documentation |
| [cli](https://github.com/open-delivery-spec/cli) | CLI — `ods validate branch|commit|pr` |
| [validate-action](https://github.com/open-delivery-spec/validate-action) | GitHub Action for CI compliance |

## Modules

**Production (L1):** Branch Naming, Commit Message, PR Description with AI Disclosure.

**Experimental (04–09):** AI Change Review, CI Failure, Release Readiness, Approval Workflow, Rollback Plan, Production Evidence. Direction-setting, not production gates yet.

See [ROADMAP](https://github.com/open-delivery-spec/spec/blob/main/ROADMAP.md).

## Quick Start

```bash
go install github.com/open-delivery-spec/cli/cmd/ods@latest
ods validate branch feature/add-oauth-login
ods validate pr --file PR_BODY.md
```

## How ODS Fits

SLSA proves how artifacts were **built**. ODS proves how changes were **delivered** — the delivery-governance layer before the build-provenance layer.

## License

Apache 2.0
