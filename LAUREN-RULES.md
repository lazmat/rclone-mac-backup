# Lauren Tan's 20 rules (the one house rule set)

These rules apply everywhere we work: the Mac harness, Atlas, Milo, dr eggbot, the Cursor app, the Cursor private worker on the Mac and the Grok CLI. mcode is banned (runaway memory). Source: `~/os-validation-2026-10-07/lauren-tan-2026-10-07.md` (her pstack playbooks at commit d0ef80d and her posts). Edit only this file. Every other place pastes it word for word or points here. Nothing goes into the system unless one of these rules needs it.

## The 20 rules

1. One coordinator plans, checks and decides. It never does the work itself; workers make every edit.
2. Every handoff is a complete brief: goal, scope (files it may write, files it must not touch), context, acceptance, verify command, timebox, forbidden actions and report shape. A brief with a missing field is not sent.
3. The numbered standing orders are pasted word for word into every new worker and every retry. Orders that live only in memory get dropped.
4. New work, a retry or a fix goes to a fresh worker with the full brief, never to a resumed one.
5. Every shared file has exactly one writer. Status files are generated from the records, never edited by hand.
6. Parallel workers never share a checkout. Each gets its own cloud agent or worktree, with one writer per branch.
7. Done means a check that can fail, run on the real artifact. A build, green tests or a marker line on its own is not proof.
8. Every verification goes into one ledger, keyed by the job (or the PR and its head commit), with one of five verdicts: live-ui-verified, unit-test-verified, type-check-only, verifier-blocked, verifier-failed. A new commit voids the old row.
9. The verifier runs on a different model family than the worker, and nothing closes or merges on its author's own verdict.
10. Any stage can stop the line, and every stage hands over evidence: the failing check and the passing check, plus logs or screenshots.
11. Retries follow the failure: out of room means a smaller scope, a network drop retries as is, a tool error tries another model, an unknown failure retries once. Lauren allows two retries at most; this house allows one, then the unit stops as BLOCKED and goes to Lazaros. A retry gets the original brief plus the failure, never a hint about how to pass the check.
12. Progress is judged only by side effects: commits, files, ledger rows, PRs. Never resume or ping an agent to ask how it is going; one past its time with no side effect is stuck and gets replaced.
13. A long or unattended run keeps an append-only decision log (time, phase, decision, why, evidence, result), and a different model family audits it.
14. Reversible work goes ahead without asking. Irreversible actions always stop for Lazaros's yes: deploys, live data writes or deletes, force-push, spend, orders and messages to anyone. An open question is parked with a default and the work goes around it.
15. A mistake that repeats becomes a check (structure, type, lint or test), not more prose. Each rule names the check that enforces it.
16. The coordinator keeps its context clean: bulk work and raw output go to workers that return summaries. Every unit is small and has a turn or time budget sized to the work.
17. A loop has a pass or fail finish condition, not a duration, and runs unattended only after it has shown it can verify its own work.
18. Every claim in a report carries its evidence (path, command, output) or a label: measured, inferred or guess. Never invent a link, a number or a citation.
19. Which model does which role lives in one config. Cheap, fast models do mechanical work; the strongest model does the hard judgment.
20. Gathering context (inbox, chats, backlog) is separate from doing the work. Intake routes work to a project; that project's coordinator runs it.

## Rules 21 to 30 (added 2026-10-09 from Lauren Tan's pstack playbooks)

Each names the check that enforces it (rule 15). "OPEN" means the check does not exist yet.

21. Every app has a verify skill: a project-local CLI (`doctor`, `launch`, `drive`, `screenshot`, `cleanup`) and a Feature Map, one file per feature, that drives the real app the way a user does. It is rebuilt or checked daily. A feature not in the map is not verified. *Check: `gate:ui` fails if a view in the app has no Feature Map entry or the doctor step fails. Owner: Facade. Status: OPEN until gate:ui lands.*
22. The live lane is the floor. A verdict needs a live run on the real surface (Chromium and WebKit) at 1440, 1280, 1024, 800 and 390. Unit tests and type checks alone give only unit-test-verified or type-check-only, never a pass for merge or deploy. Fresh verifiers rerun at every push that changes the patch. *Check: the ledger refuses live-ui-verified without a live run; the deploy script reads only that verdict. Status: ledger enforced, live run part OPEN until gate:ui lands.*
23. Land one at a time. Each PR is verified on its own exact head. Landing is one PR at a time, bottom-up; after each merge, rebase, re-verify and recheck the next. Never merge or deploy a batch. A rebase that changes the patch voids the verdict. *Check: `scripts/deploy-app-pages.sh` refuses a build whose head has no live-ui-verified row. The "more than one new PR since the last verified build" refusal is OPEN.*
24. Small PRs, ready, with a briefing body. Narrow PRs, one purpose each, files capped (Lazaros already uses 3). The body has Why, What changed, Scope, Blast Radius and Verification. *Check: PR template with those six headings. Status: OPEN until a CI check rejects a PR body that lacks them.*
25. Every fixed bug lands its test first. Reproduce on the same surface, commit the failing repro before the fix, then the fix. Tests assert what the user sees against a literal value; a test that passes when its imports return nothing is rewritten or deleted. *Check: PR template and CI require a test commit before the fix commit. Status: OPEN until CI has it.*
26. Output is externalized when it lands. A finished unit commits, pushes and opens its PR in the same turn. Work that exists only on one machine is not done. *Check: the Grok CLI Stop hook blocks finishing with uncommitted or unpushed changes. Status: OPEN until the hook is installed (guard config is Lazaros's by hand).*
27. Audit tick by side effects. A scheduled zero-token script (launchd, hourly) lists branches, PRs without a verdict for their head, ledger rows and idle units, and writes a file or notifies only when a unit is past its time. It never messages an agent to ask and never wakes a model to ask. This amends rule 12. *Check: the script and its log. Status: OPEN until installed.*
28. Stop means zero writes. A hold or stop from Lazaros reaches every worker as a stop-writing order at once, and stays until he releases it. *Check: one stop file that every worker wrapper and the Stop hook read before any write. Status: OPEN.*
29. Pause and pickup. On pause, restart or low context: finish or back out the step, make a `wip:` commit, write a resume note. A fresh worker resumes from the note and the branch, not from the old agent. *Check: the Stop hook requires a wip commit and a resume note. Status: OPEN.*
30. Candor and scope. A worker or coordinator may say no, push back, or say "this doesn't earn its place". A discovery made mid-run that does not block the unit goes to backlog, not into the PR. *Check: none mechanical; reviewed in the audit of the decision log (rule 13).*

## Amendments to existing rules (the 20 above stay word for word)

- Rule 7 also: a fixed UI bug needs before and after screenshots or video from the real surface.
- Rule 12 also: see rule 27. No agent polls or asks another agent for status; the zero-token hourly script is the only thing that looks, and it never wakes a model to ask.
- Rule 15 also: fix at the highest level that works: remove the cause by structure, then types, then lint or CI, then a test; write prose last. A new check must be shown to fail on a real past mistake. Keep one table pairing each rule with its enforcer: `docs/RULES-ENFORCEMENT.md`.
- Rules 16 and 2 also: land verified work by about 70% of the time budget; do not start new units after that.

## Kept on purpose (do not "fix" these)

- Rule 14 stays: no auto-merge after a veto window (Lauren merges after 1 hour). Lazaros says yes to merges and deploys. An irreversible action needs a yes; autonomy is earned. Revisit once rules 21 to 23 have run clean for several weeks.
- Rule 11 stays: one retry, then BLOCKED. Stricter than her two retries.
- Local first on the Mac worker stays over her cloud-first default, for cost and for his approval rules.

## How they apply here (each line names its rule)

- Unclear, risky or principal-only items go to Lazaros in one bundle, with a recommended default. (11, 14)
- No live orders or trades. NAV never trades. ORB-MNQ is frozen. (14)
- No deploys, live SQL, spend or outbound messages without Lazaros's yes. Exception: the DSY desk's build mode on DSY's own Supabase (SQL, migrations, edge deploys, branches). A DSY production release to paying users still needs his yes. (14)
- Secrets live only in the macOS Keychain and are read at run time. Never put them in files, logs, commits, briefs or chat, and never print them. (14: a leaked secret cannot be taken back)
- Other agents work in the same checkouts: never `git add -A`, `git add .`, `git commit -a`, force-push, `stash`, `reset --hard` or `clean`, and never touch a file you did not change. Stage by explicit path. (5, 6, 14)
- `DECISIONS.md` is owner only. The Mac harness (`AI-OS/Runtime/remote-exec/`) changes only through its staged process. Guard config (`.cursor/hooks*`) changes only by Lazaros, by hand. (5, 15)
- A verify check reads only files the worker can see (inside the job folder, or committed). It tests a pattern or a fact, not a value that can move. The brief and the check name the same thing. (7)
- Model roles: on the Mac, `AI-OS/Runtime/models.toml`. In Cursor: composer-2.5 or grok-4.7, standard (never Fast), medium effort, never Opus; the Other Models pool only for occasional review. The verifier's family differs from the builder's. (19, 9)
- Work runs on the Cursor private worker on the Mac, each unit in its own fresh GitHub clone, as many in parallel as the Mac has room for (check free memory and swap before heavy steps). (6, 16)
- First understand the task, break it into independent sections, and run them in parallel as separate agents on separate branches to finish faster, without compromising quality. (6, 16)
- On the Mac, the Grok CLI builds and Composer verifies. (9)
- On Cursor cloud agents, Composer builds and the Grok CLI verifies. (9)
- Builder and verifier are always two different models. (9)
- mcode is banned. (9)
- Cloud agents only when Lazaros asks for one. (14)
- A Mac job's finish (its PR and its agent report) is what wakes the coordinator. No agent polls or asks another agent for status. One zero-token shell script (launchd, hourly) may list unpushed commits, open PRs without a verdict for their head, and idle units, and write a file or notify only when a unit is stuck; it never wakes a model to ask. (12, 27)
- Bug to test: every bug fixed gets a permanent test or check, committed as a failing repro before the fix. Extends rule 15. (15, 25)
- Release gate: `npm run gate:ui` must pass on the exact build (Chromium and WebKit at 1440, 1280, 1024, 800 and 390) before any deploy, and `scripts/deploy-app-pages.sh` refuses to run without that result for the same build hash. No skip flag. A deploy still needs Lazaros's yes. (7, 8, 10, 14)
- Lessons file: `docs/LESSONS.md` is read at the start of every job. After an incident, add what happened and the check that now prevents it, or mark it OPEN. (3, 15)
- If a guard blocks you, stop and report what you tried. Do not reword the command to get around it. (10, 14)

## KEEP-REVIEW (Lazaros ordered these; no Lauren rule requires them; his call)

- Privacy zones (Health, Finance, Natalia, `Personal/`, Dropbox, `~/.ssh`) are not read by agents. On 8 Oct he allowed personal files into his private GitHub repos; never into the repo shared with Adonis.
- Altamare: never commit `Personal Details/`.
- Briefs and reports: plain words, short sentences, no long dashes.
- Every bot tells NAV about every expense.
