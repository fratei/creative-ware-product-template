# {{PRODUCT_NAME}} — Agent Operating Entry Point

This repository supports autonomous execution by Copilot and GitHub Actions agents.

## Scope
- Product feature implementation, CI/CD, and configuration changes live here.
- Company-wide strategy and orchestration stays in [creative-ware-hq](https://github.com/fratei/creative-ware-hq).
- Product registry updates (e.g., launch status) go in `products/{{PRODUCT_SLUG}}.md` in HQ.

## Required agent behavior
1. Read `.github/copilot-instructions.md` first — it describes architecture, conventions, and important files.
2. Prefer minimal, reversible changes.
3. Never manually edit `strategy/agent-state/*.json` — these are workflow-managed.
4. Keep all `{{PLACEHOLDER}}` tokens intact in template files that are not yet bootstrapped.
5. Run existing lint/test commands before proposing changes (check for `package.json` first).

## Cross-repo autonomy dependencies
- `AGENT_PAT` (repo/org secret) required for cross-repository automation triggered from HQ.
- Optional MCP and third-party integrations must be configured via repository/org secrets and allow-lists (see `docs/agents/AUTONOMY-ADMIN-CHECKLIST.md`).

## Fast start
- Dev container: `.devcontainer/devcontainer.json`
- Copilot cloud setup: `.github/workflows/copilot-setup-steps.yml`
- Agent handbook: `docs/agents/HANDBOOK.md`
- Agent dashboard: `docs/agents/DASHBOARD.md`
