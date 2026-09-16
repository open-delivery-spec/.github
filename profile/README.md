<div align="center">

# Open Delivery Spec

### Zero-config governance and visibility for AI-assisted code

**Claude Code, GitHub Copilot, and Cursor already stamp `Co-Authored-By` trailers on every commit.**
ODS reads them automatically in CI — showing how much of your delivery is AI-assisted,
routing review attention to the changes that need it, and enforcing your policy before merge.

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
 ①  Detect   →  Which changes are AI-assisted?     (Co-Authored-By trailers, git-ai notes, PR disclosure, branch prefix)
      │
      ▼
 ②  Analyze  →  What did the checks find?          (built-in AI heuristics + your scanners' SARIF)
      │
      ▼
 ③  Score    →  How much technical debt is added?  (driven by quality, amplified by AI share)
      │
      ▼
 ④  Check    →  Does it meet your policy?          (OPA Rego: pass, warn, block + a review tier)
      │
      ▼
  PASS · WARN · BLOCK   +   PR comment · job summary · HTML report · evidence document
```

ODS is a **signal producer, not a quality oracle**: attribution reflects what the
tools disclose, a `PASS` means no deny rule fired, and no number claims code is
correct.

## Quick start

Add [`open-delivery-spec/validate-action@v1`](https://github.com/open-delivery-spec/validate-action#quick-start)
to your pull-request workflow; the whole setup is one job, and the Action's
README has it ready to paste. Prefer the CLI, or want it locally too?

```bash
go install github.com/open-delivery-spec/cli/cmd/ods@latest
ods init   # writes the CI workflow and .ods/policy.rego (the built-in default, to edit)
```

The [Get Started](https://open-delivery-spec.github.io/spec/get-started.html)
guide covers rollout and policy customization.

## The organization view

One repository answers "how much of this project is AI-assisted". The
[`org-ai-report`](https://github.com/open-delivery-spec/.github/blob/main/.github/workflows/org-ai-report.yml)
reusable workflow answers it for every repository at once: on a schedule it
scans them, merges the results with `ods report merge`, and publishes one
dashboard as an artifact, a job summary, or GitHub Pages. Nothing leaves your
GitHub account.

```yaml
jobs:
  ai-report:
    uses: open-delivery-spec/.github/.github/workflows/org-ai-report.yml@main
    permissions:
      contents: read
```

That covers every repository of the organization it runs in; `with:` takes
`org`, `repos`, `since` and `deploy-pages` when you want something else. We run
it on ourselves: [latest run](https://github.com/open-delivery-spec/.github/actions/workflows/org-ai-report.yml).

Guide: [Organization-wide View](https://open-delivery-spec.github.io/spec/org-view.html).

## Repositories

| Repo | What it is |
|------|------------|
| 📘 [**spec**](https://github.com/open-delivery-spec/spec) | The specification: contracts (JSON Schemas), the conformance suite, policy templates and the docs site |
| ⚙️ [**cli**](https://github.com/open-delivery-spec/cli) | Go CLI — `ods detect · analyze · score · check · report · attest · rules · init` |
| 🤖 [**validate-action**](https://github.com/open-delivery-spec/validate-action) | One-step GitHub Action wrapping the full pipeline |
| 🏢 [**.github**](https://github.com/open-delivery-spec/.github) | This profile and the `org-ai-report` reusable workflow |

## Where ODS fits

**OpenSSF Scorecard** covers supply-chain practices and **SLSA** artifact
provenance; ODS covers the gap between them: which changes were AI-assisted,
whether they met your policy, and whether you can show it. It consumes AI code
reviewers' verdicts rather than competing with them. See
[Ecosystem](https://open-delivery-spec.github.io/spec/ecosystem.html) and
[ODS and SLSA](https://open-delivery-spec.github.io/spec/comparison/slsa.html).

<div align="center">

Apache 2.0 · Built in the open

</div>
