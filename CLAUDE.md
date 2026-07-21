# CLAUDE.md

## What this repo is
The GitHub profile repo: one README, the public index of the repos. The README is generated from stack-data's repo registry (`data/repos.json`); the footer carries the last-built date.

## How to edit
Fix facts in the registry first, then rebuild the README from it. Hand edits here drift the next time the index is rebuilt.

## Rebuild recipe
The exact sequence, for a successor or a fresh machine (the retiring-engineer audit asked for this in writing):

```bash
cd ../stack-data
bash scripts/sd-readme            # renders dist/profile-README.md, public repos only
cp dist/profile-README.md ../risaac09/README.md
cd ../risaac09
git diff                          # read it; the registry is the source of truth
git add README.md && git commit -m "Rebuild README from registry" && git push
```

Never run `sd-readme --include-private` and push the result; the profile page is public and the private flag exists to keep client and strategy repo names off it. The curated intro lives in stack-data's `profile/header.md` and survives regeneration; edit it there.

## Routing
- Tier: none, a single public README. The spine is stack-data, Tier 1, the operational source of truth, a sibling clone (`../stack-data`).
- No phase-zero kit is deployed here, on purpose; the six trigger phrases and "log learnings" do not run in this repo.
