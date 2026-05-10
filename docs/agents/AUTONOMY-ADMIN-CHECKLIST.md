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
