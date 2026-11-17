# Security Policy

## Reporting a vulnerability

Please **do not open a public issue** for security problems.
Use GitHub's private reporting instead: *Security* tab → *Report a vulnerability*
(or contact the maintainers directly). We aim to acknowledge reports within 3 working days.

## What is checked automatically

Every pull request, every push to the default branch and a weekly scheduled run execute
`.github/workflows/security.yml`:

| Check | Tool | Blocks merge |
|---|---|---|
| Leaked credentials in git history | TruffleHog | yes |
| Vulnerable dependencies (HIGH/CRITICAL, fix available) and secrets | Trivy | yes |
| IaC / Dockerfile / Kubernetes misconfigurations | Trivy | yes |
| Insecure code patterns (XSS, injection, unsafe APIs…) | Semgrep | yes |
| Advisories in production npm dependencies | npm audit | yes |
| AI review of the PR diff for security bugs | Claude Code Security Review | comments on the PR |

Dependabot (`.github/dependabot.yml`) opens weekly update PRs for dependencies and for the
pinned GitHub Actions.

## Rules for contributors

- Never commit secrets (passwords, API keys, tokens, private keys). Use CI/CD credentials (Jenkins credentials, Kubernetes Secrets).
- If a secret was ever committed, **rotate it** – deleting it from the latest commit is not enough,
  it stays in git history.
- Keep third-party GitHub Actions pinned to a full commit SHA.
- Container images must not run as root and must use a supported Node.js base image.

## Recommended repository settings (done once, in GitHub UI)

- *Settings → Branches*: protect the default branch, require PR review and the **Security** checks.
- *Settings → Code security*: enable Dependabot alerts, secret scanning and push protection
  (free for public repos; requires GitHub Advanced Security for private ones).
- *Settings → Secrets and variables → Actions*: add `ANTHROPIC_API_KEY` to enable the AI review job.
