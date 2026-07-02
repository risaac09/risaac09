# risaac09

The GitHub profile repo (risaac09/risaac09). Its single `README.md` renders as Isaac's profile home at github.com/risaac09, the first page a stranger sees before any repo. The README is a generated artifact, built in stack-data (Tier 1, the spine, a sibling clone at `../stack-data`) by `scripts/sd-readme` from the repo registry and a curated intro, then pushed here by hand. This repo holds no tier of its own. It is a public rendering surface over the Tier 1 registry, comparable to a Tier 2 view but aimed at GitHub itself rather than at a PWA.

## What it is (technical)

One file, `README.md`. There is no code, no `.github/workflows/`, no `.claude/` kit, no CLAUDE.md.

The source of truth lives upstream in `../stack-data`:

- `data/repos.json`: the repo registry, one row per repo with machine facts from `gh repo list` plus curated fields (category, status, notes). This repo's own row is `r-risaac09` (visibility PUBLIC, category infra).
- `profile/header.md`: the curated intro. Created once by the generator, then hand-edited; regeneration preserves it.
- `scripts/sd-readme`: the generator. Renders non-gone repos grouped by category, active first, behind the header, and writes `dist/profile-README.md`. PUBLIC visibility only by default; `--include-private` exists for other uses and must never feed this repo. The script rewrites em-dashes in registry descriptions to commas so the output passes the voice rules.

The generator's header comment and `../stack-data/docs/SCRIPTS.md` (the sd-readme and sd-repos entries) document the mechanics. This file points there instead of restating the jq so the prose cannot drift from the code.

## How it runs (operational)

Nothing runs automatically. No Action in this repo or in stack-data regenerates or pushes the README; the single commit here was a manual push. The full loop is the runbook in Workflows below: refresh the registry with `sd-repos`, regenerate with `sd-readme`, copy `dist/profile-README.md` here as `README.md`, commit, push to main. The footer's "Last built" date is the freshness marker; when it lags the latest `repos.json` change, rerun the loop.

The one hard rule: never hand-edit `README.md` here. The next regeneration-and-push overwrites the edit silently. Intro copy changes go to `../stack-data/profile/header.md`; repo blurbs and status flags go to the matching row in `../stack-data/data/repos.json`.

## Why it exists (intellectual)

The registry in stack-data already holds one row per repo, refreshable from GitHub and linkable into the entity graph. The profile README is a public view rendered off that registry, so the profile page stays accurate by construction instead of by memory. The intro lives in `profile/header.md` rather than in this repo for one reason: it has to survive regeneration, and anything typed here would not. The thinking is written in the header comments of `../stack-data/scripts/sd-repos` and `scripts/sd-readme`.

## How it works (methodological)

Registry-driven generation with a curated header. Machine facts refresh from GitHub via `sd-repos`, which preserves hand-curated fields across runs and flags vanished repos `gone:true` without deleting them. `sd-readme` then renders the public rows; category order and active-first sorting are encoded in the script's jq, not maintained as prose anywhere. A one-file output repo needs no methodology doc beyond this paragraph; writing one would be apparatus ahead of behavior.

## How it speaks (marketing and comms)

This repo is a marketing surface, the top of the GitHub funnel. The positioning copy ("Facilitator, filmmaker, ultrarunner, builder... voice lab for mission-driven professionals") lives in `../stack-data/profile/header.md` and lands here verbatim; the repo links below it are the funnel, and no separate funnel doc is needed.

Two constraints govern every word that surfaces here:

- The estate voice rules in `../stack-data/CLAUDE.md` (no em-dashes, no rule-of-three, no jargon). They apply to `header.md` and to every registry description; the generator already converts em-dashes in descriptions.
- The rp-intranet boundary. The page is public, so registry descriptions of public repos carry no client names, no pricing, nothing from rp-intranet, and no `--include-private` build ever lands here.

## Where it goes (strategic)

Untiered public infra, status active per its registry row (`r-risaac09` in `../stack-data/data/repos.json`, the only place its role is recorded). It is deliberately absent from the phase-zero kit's ten consuming repos listed in `../rubinstein-productions-toolkit/CLAUDE.md`; do not deploy `.claude/` here or add it to `install.sh --all` without a decision upstream.

Gap: whether the regenerate-and-push loop should ever be automated (for example inside stack-data's weekly-sync) is an open decision recorded nowhere; the answer would come from Isaac, weighed against the behavior-before-apparatus corrective in stack-data's CLAUDE.md.

Gap: this repo has no CLAUDE.md, so a Claude session opened here has only the README footer as a hint that the file is generated; whether to add one (the audit's proposed short guardrail file) is Isaac's call.

## Workflows

Automated: none. No GitHub Actions, hooks, cron, or launchd in this repo, and no stack-data workflow references sd-readme or profile-README. No secrets are needed.

Manual, the publish loop, run whenever the registry or intro changes:

1. Refresh the registry. In `../stack-data` run `scripts/sd-repos`, then curate any new rows (category, status, description) in `data/repos.json`.
2. Edit copy if needed. Intro: `../stack-data/profile/header.md`. Blurbs: the description fields in `data/repos.json`.
3. Regenerate. In `../stack-data` run `scripts/sd-readme` (no flags; public-only is the default and the requirement). Output lands at `dist/profile-README.md`.
4. Publish. Copy `../stack-data/dist/profile-README.md` into this repo as `README.md`, commit, push to main. No script performs this step; the generator's final echo names it.

Good looks like: `dist/profile-README.md` and this repo's `README.md` byte-identical, the footer's "Last built" date current, and every listed repo public with a clean description. As of 2026-07-02 the two files match and a fresh run changes only the date line.

## Known drift

For Isaac to rule on; fixes belong upstream, not in this repo.

- Freshness: the README footer says "Last built 2026-06-16", sixteen days stale as of 2026-07-02. Content still matches the registry, so this is freshness drift only, but the loop has not run since the late-June registry commits.
- alchemy-diagnostic: the README lists it as a parked repo, while `../alchemy/CLAUDE.md` says it was consolidated into alchemy on 2026-06-22 and should be archived. The registry row (`gone:false`, status parked) is the stale source; retire the row in `../stack-data/data/repos.json`, then regenerate.
- three-type-evaluation: the registry marks it PRIVATE, while its own CLAUDE.md and the toolkit's frame it as a public methodology paper. If it went public, the registry row is stale and the repo is missing from this profile page; a one-time `gh` check upstream settles it.
