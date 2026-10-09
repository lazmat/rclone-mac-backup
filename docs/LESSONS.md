# Lessons (read at the start of every job)

Rules 3 and 15. Every job reads this file first. After an incident, add an entry: what happened, and the check that now prevents it, or OPEN if no check exists yet. A check must be shown to fail on the real mistake. Newest first.

## 2026-10-09: a combined deploy broke four things at once

- **What happened:** several UI PRs were combined and deployed together. Overview scroll broke, Items rows jittered, the toolbar overlapped, and the Sequence sub-tabs broke. Nobody had verified the combined build on its own head.
- **Check that prevents it:** `scripts/deploy-app-pages.sh` refuses a deploy unless a passing gate stamp exists for the exact head and dist hash, with no skip flag (rules 22, 23). The gate itself, `npm run gate:ui` (Chromium and WebKit at 1440, 1280, 1024, 800, 390), is owned by Facade. **Gate: OPEN until it lands. Guard: enforced.**
- **Still OPEN:** the refusal of more than one new PR since the last verified build (rule 23).

## 2026-10-09: Plan endless scroll returned

- **What happened:** the Plan endless-scroll bug was fixed once and came back, because the fix had no permanent test.
- **Check that prevents it:** every fixed bug lands a failing test first (rule 25) and the Plan scroll check belongs in `gate:ui`. **OPEN** until the gate has the Plan scroll invariant and CI requires the test commit before the fix commit.

## 2026-10-09: finished work sat idle for hours

- **What happened:** units finished and stayed unpushed or unmerged while nobody noticed.
- **Check that prevents it:** rule 26 (Stop hook blocks finishing with uncommitted or unpushed work) and rule 27 (hourly zero-token script lists unpushed commits, PRs without a verdict for their head, idle units). **OPEN**: neither is installed yet.

## 2026-10-09: mcode runaway memory

- **What happened:** mcode runs used memory without limit and slowed the whole Mac.
- **Check that prevents it:** mcode is banned (`LAUREN-RULES.md`); the Grok CLI builds, and every heavy step checks free memory first. **OPEN** as a mechanical check: nothing yet blocks launching mcode.

## 2026-10-08: a backup script rotation deleted a hand-made bundle

- **What happened:** `ceo-git-bundle-backup.sh` rotated `ceo-*.bundle` with KEEP=1. It matched the hand-made `ceo-PRE-GITHUB-20261008T1922.bundle` and deleted it. The history was rebuilt from pinned refs.
- **Check that prevents it:** rotation now matches only the names the script mints (`ceo-YYYYMMDDTHHMMSSZ.bundle`). A dry check listed what would be deleted and showed the cutover bundle name cannot match. The old history is also pinned as `refs/pre-github/*`. **Enforced.**
