# Contributing to Open Delivery Spec

This is the organization default, shown for every `open-delivery-spec`
repository that has no CONTRIBUTING.md of its own. The process (issue, discuss,
pull request), what to contribute and the design principles are in the
[spec repository's CONTRIBUTING.md](https://github.com/open-delivery-spec/spec/blob/main/CONTRIBUTING.md);
the conventions below apply everywhere.

## Branches and commits

- Never push to `main`; every change enters through a pull request.
- Branch names follow [Conventional Branch](https://conventional-branch.github.io/)
  (`feature/`, `bugfix/`, `hotfix/`, `release/`, `chore/`; AI-agent branches
  `claude/`, `copilot/`, `cursor/` are accepted).
- Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/),
  subject at most 80 characters. CI enforces both with `commit-check`.

## AI-assisted contributions

AI assistance is welcome on three conditions. Every repository here runs the
[open-source disclosure policy](https://open-delivery-spec.github.io/spec/oss-ai-policy.html)
on its own pull requests, so CI checks what it can see of them and posts the
result as a comment.

1. **Disclose it.** Keep the `Co-Authored-By` trailer your tool adds (Claude
   Code, Copilot and Cursor add one on their own), add an
   `Assisted-by: <tool>` trailer, or tick the AI Disclosure box in the pull
   request template. A change that looks AI-assisted but says nothing gets a
   nudge in the comment and a maintainer's look; it is never blocked on
   suspicion.
2. **Test it.** AI-authored code comes with tests. Source added without a
   test is flagged.
3. **Own it.** You have read and understood what you submit and can answer
   questions about it in review. Changes to CI, dependencies or
   security-sensitive paths get extra eyes.

## License

By contributing, you agree that your contributions are licensed under the
[Apache License 2.0](https://github.com/open-delivery-spec/spec/blob/main/LICENSE).
