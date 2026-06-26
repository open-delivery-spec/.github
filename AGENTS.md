# Agent Development Rules — open-delivery-spec org

This file instructs AI coding agents (Claude Code, Copilot Workspace, etc.) on how to work across all repositories in the `open-delivery-spec` organization.

Each repository also has its own `AGENTS.md` with repo-specific rules. This file covers the shared baseline that applies everywhere.

## Repositories

| Repo | Purpose |
|------|---------|
| `open-delivery-spec/cli` | The `ods` CLI — detect, analyze, score, check |
| `open-delivery-spec/spec` | The ODS specification, schemas, conformance tests |
| `open-delivery-spec/validate-action` | GitHub Action wrapper around the CLI |
| `open-delivery-spec/.github` | Org-level workflows and community health files |

## Branching Rules

- **NEVER push directly to `main`.** All changes enter via pull request only.
- **Always start from the latest `main`.** Before creating any branch:

  ```bash
  git fetch origin
  git checkout main
  git pull origin main
  git checkout -b <branch-name>
  ```

  Never branch from a stale local `main` or from another feature branch.

- **Branch names must follow [Conventional Branch](https://conventional-branch.github.io/) naming.** Allowed prefixes:

  | Prefix | Use for |
  |--------|---------|
  | `feature/` | New features |
  | `bugfix/` | Bug fixes |
  | `hotfix/` | Urgent production fixes |
  | `release/` | Release preparation |
  | `chore/` | Maintenance, dependencies, **tests, docs, CI, refactors** |

  AI-agent branches are also accepted: `claude/`, `copilot/`, `cursor/`, `github-actions/`

  Long-lived branches: `main`, `master`, `develop`

  Branch names must be **lowercase**. The description part after the prefix must not contain `/`.

  Good: `feature/sarif-ingestion`, `bugfix/coverage-sentinel`, `chore/add-tests`, `claude/my-task-id`
  Bad: `feature/Add-SARIF`, `feat/x` (feat is a commit type, not a branch type), `chore/my/nested/path`

> ⚠️ **Conventional _Branch_ types are NOT the same as Conventional _Commit_ types.**
> Branch names use `feature/bugfix/hotfix/release/chore`; commit messages use
> `feat/fix/docs/test/…`. Prefixes like `test/`, `feat/`, `fix/`, `docs/`, `ci/` are
> valid commit types but **not** valid branch prefixes — `commit-check` rejects them.
> For test/docs/CI/refactor work, branch under `chore/`.

## Commit Message Rules

All commits must follow the [Conventional Commits](https://www.conventionalcommits.org/) specification. This is enforced in CI by `commit-check`.

```
type(scope): description
```

- **type**: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`, `ci`, `build`, `revert`
- **scope**: optional, lowercase, no slashes (e.g., `analyzer`, `policy`, `cmd`, `action`)
- **description**: imperative mood, no capital first letter, no trailing period
- **subject line**: maximum **80 characters** (type + scope + description combined)

Examples:
```
feat: add SARIF ingestion to analyze command
fix(policy): guard coverage rules against -1 sentinel
ci: replace hand-rolled checks with commit-check-action
docs(spec): add conformance test scenarios
chore: bump opa dependency to v1.4
```

Commits that violate this format are rejected by `commit-check` in CI.

## PR Workflow

- Ensure your branch is rebased on the latest `main` before opening a PR:

  ```bash
  git fetch origin
  git rebase origin/main
  ```

- Do not open PRs without explicit user instruction.
- All PRs target `main`.
- If CI fails on a PR you created, investigate the failure and fix it.

## Language

Always reply in the same language the user writes in. If the user writes in Chinese, respond in Chinese.
