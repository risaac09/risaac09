# CLAUDE.md

## What this repo is
The GitHub profile home. One README.md that renders on github.com/risaac09. The full map is `docs/PRODUCT.md`.

## The one guardrail
Never hand-edit README.md here. It is generated output: `../stack-data/scripts/sd-readme` renders it from `data/repos.json` plus `profile/header.md`, and the next regeneration overwrites any edit silently. Intro copy changes belong in `../stack-data/profile/header.md`; repo rows change through the registry. The publish runbook is in `docs/PRODUCT.md`.

## Routing
- Tier: none, public infra outside the personal stack. The spine is stack-data, Tier 1, the operational source of truth, a sibling clone (`../stack-data`).
- No phase-zero kit is deployed here on purpose; the repo holds one generated file.
- Everything that surfaces here is public; the estate voice rules apply.
