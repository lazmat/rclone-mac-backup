# HANDOFF-OS — rclone-mac-backup

## What is done (branch `os/files-20261009`)

- Lauren OS bundle on `os/files-20261009`: LAUREN-RULES.md, docs/LESSONS.md, .cursor/rules/house-seat.mdc, .grok/hooks/lauren-guards.json, AGENTS.md.
- `LAUREN-RULES.md`, `docs/LESSONS.md` from `lazmat/digital-shipyard` main (2026-10-09).
- `.cursor/rules/house-seat.mdc` (`alwaysApply: true`); cloud: Composer 2.5 builds (Fast off), Grok 4.7 verifies (medium, never Fast); no Grok CLI in cloud.
- `AGENTS.md` — who builds, visible Cursor agents, read `docs/LESSONS.md` first.
- `.grok/hooks/lauren-guards.json` (Mac Grok CLI).
- gitleaks on OS files only.

## What is left

- **Merge:** Lazaros (Milo directs; agents do not merge).
- **Verify skill:** N/A — no build pipeline.
- **Release gate:** N/A — no build to gate.
- **Deploy guard:** N/A — no deploy artifacts.

## Exact done criteria

1. Draft PR merged to `main`.
2. OS files on `main` match house standards.
3. Build repos: verify skill + gate in `AGENTS.md` or OPEN with owner.
4. Deploy repos: guarded deploy; Lazaros yes for production.
5. gitleaks clean on OS file set.
6. No secrets / personal / CEO-only data.

## Blockers

```
None — branch pushed.
```

## Owner

- Merge / unarchive: **Lazaros**
- Skills/gates: **cloud agent** (Milo queue)
