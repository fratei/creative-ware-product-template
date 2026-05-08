# {{PRODUCT_NAME}}

> {{PRODUCT_DESCRIPTION}}

## Overview

| | |
|---|---|
| **Product** | {{PRODUCT_NAME}} |
| **Company** | [CreativeWare](https://github.com/fratei/creative-ware-hq) |
| **Owner** | [@fratei](https://github.com/fratei) |
| **Status** | 🏗️ Building |

## Getting Started

```bash
# Clone the repo
git clone https://github.com/fratei/{{REPO_NAME}}.git
cd {{REPO_NAME}}

# Install dependencies
npm install

# Run locally
npm run dev
```

## Architecture

> Document your architecture here after initial setup.

## Agent System

This product runs an autonomous agent fleet:
- **Agent Fleet** — CPO/CFO/CTO review opportunities every 2h
- **PR Pipeline** — Autonomous review + merge with risk classification
- **Observability** — Monitoring + dashboard updates every 2h
- **CI/CD** — Automated lint → test → build → deploy

See [Agent Dashboard](docs/agents/DASHBOARD.md) for live status.

## Framework Version

[![Framework Version](https://img.shields.io/badge/framework-main-orange)](./framework-version)

This template is wired to the CreativeWare reusable agent framework v1 rollout.
Until the `v1` tag is published, workflow consumers temporarily reference `@main`.
When `v1` is tagged, switch both workflow `uses: ...@main` references and config references in `config/agents.config.json` (`$schema` and `$inherit_from`) to `v1`.
After the tag is published, `template-sync.yml` (in CreativeWare HQ) will open automated upgrade PRs in derived product repositories for future framework releases.

## Links

- [CreativeWare HQ](https://github.com/fratei/creative-ware-hq)
- [Product Card](https://github.com/fratei/creative-ware-hq/blob/main/products/{{PRODUCT_SLUG}}.md)
- [Agent Handbook](docs/agents/HANDBOOK.md)
- [Strategy](strategy/README.md)

---
*Part of [CreativeWare](https://github.com/fratei/creative-ware-hq) — Autonomous AI Company*
