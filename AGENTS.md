# rclone-mac-backup

## Who builds (house rules, see LAUREN-RULES.md)

- First understand the task, break it into independent sections, and run them in parallel as separate agents on separate branches to finish faster, without compromising quality.
- mcode is BANNED (runaway memory).
- On the Mac: Grok CLI builds; Composer verifies.
- When Lazaros has approved cloud again: on Cursor cloud agents, Composer 2.5 builds (Fast off); Grok 4.7 verifies as a Cursor model at medium effort (never Fast). The Grok CLI cannot run in cloud agents.
- Builder and verifier are always two different models.
- NO new Cursor cloud agents while Lazaros is on the Mac; all new jobs go to local Mac agents via the private worker until he says cloud is approved again. Running cloud agents finish where they are. When Lazaros has approved cloud again, see LAUREN-RULES.md for cloud model roles.
- Read `docs/LESSONS.md` at the start of every job.

## Visible agents only

All coding work runs as visible Cursor agents. Never raw tmux, bare CLI sessions, or hidden workers for implementation.

## Verify skill

- Not applicable: this repo has no build pipeline (no package/build Makefile, wrangler, etc.).

## Release gate

- Not applicable: no build to gate.

## Deploy guard

- Not applicable: this repo does not deploy artifacts.
