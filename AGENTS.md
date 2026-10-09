# rclone-mac-backup

## Who builds (house rules, see LAUREN-RULES.md)

- mcode is BANNED (runaway memory).
- On the Mac: Grok CLI builds; Composer verifies.
- On Cursor cloud agents: Composer 2.5 builds (Fast off); Grok 4.7 verifies as a Cursor model at medium effort (never Fast). The Grok CLI does not run in cloud agents.
- Builder and verifier are always two different models.
- Use cloud agents without asking when the Mac is the bottleneck (swap near full or jobs queued); otherwise prefer the Mac worker.
- Read `docs/LESSONS.md` at the start of every job.

## Visible agents only

All coding work runs as visible Cursor agents. Never raw tmux, bare CLI sessions, or hidden workers for implementation.

## Verify skill

- Not applicable: this repo has no build pipeline (no package/build Makefile, wrangler, etc.).

## Release gate

- Not applicable: no build to gate.

## Deploy guard

- Not applicable: this repo does not deploy artifacts.
