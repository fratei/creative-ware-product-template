# Agent Framework Migration Notes

## Summary

This template migrates from bespoke agent logic to the CreativeWare reusable framework consumers.

- Workflow file count remains **4** (`ci-cd.yml` plus 3 agent workflows), but the three agent workflows are now thin reusable-workflow consumers.
- Owner routing moved from workflow hardcoding to config (`config/agents.config.json`).
- Agent state moved from markdown (`strategy/agent-state/metrics.md`) to schema-aligned JSON files.
- Opportunity routing now supports committee-vote flow for low/medium opportunities via the framework (instead of routing every opportunity directly to owner approval).
- `product-agent-fleet.yml` schedule is now every 6 hours (`15 */6 * * *`) to match the reusable framework consumer baseline.
- `pr-review.yml` now includes a 30-minute schedule in addition to PR events so pending PRs are re-evaluated by the reusable pipeline.

## Framework references

- Framework PR: [fratei/creative-ware-hq#130](https://github.com/fratei/creative-ware-hq/pull/130)
- Framework release notes: [framework/RELEASE.md](https://github.com/fratei/creative-ware-hq/blob/main/framework/RELEASE.md)

## Temporary pin note

At migration time, `fratei/creative-ware-hq` did not yet expose a `v1` git tag, so reusable workflow references are temporarily pinned to `@main` with TODO markers in each migrated workflow.
