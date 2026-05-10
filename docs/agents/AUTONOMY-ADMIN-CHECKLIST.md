# Autonomy Admin Checklist

Use this checklist when setting up a new CreativeWare product repository for fully autonomous agent operation.

## 1) Agent instructions and setup files
- [ ] `AGENTS.md` exists and placeholders have been replaced (no `{{PRODUCT_NAME}}` literals remaining).
- [ ] `.github/copilot-instructions.md` exists and is populated with product-specific context.
- [ ] `.devcontainer/devcontainer.json` exists with product name replaced.
- [ ] `.github/workflows/copilot-setup-steps.yml` exists and runs successfully on default branch.

## 2) Branch protections and merge policy
- [ ] Require PR for default branch (`main`).
- [ ] Require required status checks before merge (at minimum: `pr-checks` from `ci-cd.yml`).
- [ ] Restrict force pushes/deletions on default branch.
- [ ] Enable auto-merge for qualifying low-risk agent PRs.

## 3) Required secrets and environment
- [ ] `AGENT_PAT` configured with cross-repo read/write scope.
- [ ] Product deployment secrets configured (e.g., `VERCEL_TOKEN`, `RAILWAY_TOKEN`, etc.).
- [ ] `copilot` environment created with required vars/secrets if needed.

## 4) MCP / third-party integration readiness
- [ ] MCP tools enabled in repository/organization settings for agents that need them.
- [ ] GitHub Actions policy allows reusable workflows from `fratei/creative-ware-hq` (required for `product-agent-fleet`, `observability-agent`, and `pr-review`).
- [ ] Third-party API keys stored as secrets (never committed to repo).
- [ ] Least-privilege token scopes verified.

## 5) Network/firewall allow-list (if applicable)
- [ ] Allow GitHub Actions standard endpoints.
- [ ] Allow Copilot cloud agent endpoints:
  - `uploads.github.com`
  - `user-images.githubusercontent.com`
  - `api.individual.githubcopilot.com` / `api.business.githubcopilot.com` / `api.enterprise.githubcopilot.com` (as applicable)

## 6) Parallel PR stability controls
- [ ] Concurrency groups configured for long-running workflows.
- [ ] Workflow/job timeouts configured to avoid hangs.
- [ ] Queue semantics (`cancel-in-progress: false`) used where in-flight work must finish.

## 7) Post-bootstrap validation
- [ ] Run `agent-readiness-audit` workflow in `creative-ware-hq` and confirm this repo shows all ✅.
- [ ] Trigger `product-agent-fleet` manually and confirm it runs without errors.

## 8) Phase 2 outcome audit (2026-05-10)
- [x] Required bootstrap files are present and wired: `AGENTS.md`, `.github/copilot-instructions.md`, `.devcontainer/devcontainer.json`, `.env.template`, `config/agents.config.json`, and all expected workflows/templates.
- [x] Parallel PR safety controls are present (`concurrency` configured with queue semantics on long-running workflows).
- [x] CI robustness for template repos is in place (`ci-cd.yml` and `copilot-setup-steps.yml` skip Node steps when no `package.json`/`package-lock.json` exists).
- [x] Resolve recurring workflow `startup_failure` on reusable-workflow consumers (examples: runs `25630548099`, `25632618240`). Fixed in Phase 3: `pr-review.yml` was missing required `owner`/`repo` inputs.
- [ ] Remove manual workflow approval friction causing `action_required` runs on Copilot PR branches (example: run `25634384981`).
- [ ] Create and track follow-up tasks/issues for remaining admin work:
  - [ ] `[AUTONOMY] Enable reusable workflow access from fratei/creative-ware-hq`
  - [ ] `[AUTONOMY] Remove manual approval gate for trusted Copilot-triggered workflows`
  - [ ] `[AUTONOMY] Run HQ agent-readiness-audit and attach evidence links`

## 9) Phase 3 implementation (2026-05-10)

### Root causes identified
- **`startup_failure` on `pr-review.yml`** — Root cause confirmed: the reusable workflow
  `reusable-pr-pipeline.yml` declares `owner` and `repo` as **required** inputs, but
  `pr-review.yml` was not passing them. Fixed by adding `with: owner/repo` to the call.
- **`action_required` for Copilot-triggered PRs** — GitHub Actions policy requires manual
  approval for workflows on PRs from first-time or external contributors. This is an admin
  setting that cannot be fixed via workflow YAML alone (see admin steps below).

### Code changes applied in Phase 3
- [x] `pr-review.yml` — Added required `owner` and `repo` inputs to the reusable-workflow call, resolving `startup_failure`.
- [x] `autonomy-self-check.yml` — New weekly scheduled workflow: validates bootstrap file presence, detects unresolved placeholders in config/workflow files, validates agent state JSON, and opens a deduplicated issue on failure.

### Remaining admin actions (cannot be automated via code)
- [ ] **Fix `action_required` for Copilot PRs**
  - Go to: _Settings → Actions → General → Fork pull request workflows_
  - Set to: **"Require approval for first-time contributors who are new to GitHub"** (or less restrictive)
  - This allows Copilot-triggered PRs from `copilot/*` branches to run CI without manual approval.
- [ ] **Verify reusable workflow cross-repo access**
  - Go to: _Settings → Actions → General → Allow GitHub Actions to create and approve pull requests_ → ✅ Enabled
  - Confirm `fratei/creative-ware-hq` is listed as an allowed source for reusable workflows (if using an org, check Org-level Actions policy).
- [ ] **Create GitHub label `autonomy/self-check`** (required by new `autonomy-self-check.yml`)
  - Run: `gh label create "autonomy/self-check" --color "0075ca" --description "Autonomy readiness self-check failure"`
- [ ] **Run HQ `agent-readiness-audit`** and attach evidence links here once passing.
