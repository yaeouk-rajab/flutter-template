# Dependabot status

_Last updated: 2026-10-27. Starter template extracted from `bookslot-mobile`; update when the state changes._

## Configuration

- Ecosystems covered: pub (`/`), gradle (`/android`), github-actions (`/`).
- Grouping: minor and patch updates grouped as `minor-and-patch`.
- Schedule: weekly.
- Ignore rules: none.

## State at last update

- Open Dependabot PRs: 0 (each merged or closed only after reading its checks).
- Default-branch CI: green at last check.
- Repository settings (read live 2026-10-10): public, `is_template` true, description set, default branch `main` (the only branch kept), secret scanning, push protection and Dependabot security updates enabled, dependency graph on (SBOM endpoint 200). Non-provider secret patterns and validity checks stay off: GitHub does not offer them on this user-owned public repo.
- Set by the repo owner through the API on 2026-10-10 (the session's proxy blocks repository-settings writes) and read back by the session the same day: private vulnerability reporting enabled (PUT 204), `delete_branch_on_merge` on (PATCH 200), repository topics set (PUT 200). Fork-PR workflow approval is `all_external_contributors` (read with the owner's token). Stale branches left over from squash-merged PRs were deleted after a fresh diff against `main` showed their content already on `main`.
- Alerts read 2026-10-10 with the repo owner's token: No Dependabot, code-scanning or secret-scanning alerts.
- Rescan 2026-10-11 repeated every dimension (open PRs, branches, CI on `main`, required contexts against the last merged PR's check runs, protection, `security_and_analysis`, private vulnerability reporting, dependency graph, `is_template`, description, topics, licence placeholders) and added one: whether the GitHub description actually describes this repo. Live result: no open PRs, `main` the only branch, CI on `main` green, every required context produced green by the last merged PR, protection unchanged, secret scanning, push protection and Dependabot security updates enabled, private vulnerability reporting on, SBOM 200, `is_template` true, description accurate, topics set, `LICENSE` filled in (2026, arb-rajab). The repo owner's token read 0 open Dependabot, code-scanning and secret-scanning alerts and fork-PR approval `all_external_contributors`. No new gaps.
- Rescan 2026-10-27 (a new loop after a gap in which Dependabot opened a fresh batch of PRs). Merged (squash) after reading each PR's check runs: #5, #7. Merged from this session: #10 (Dependabot ignore for flutter_local_notifications majors). Closed unmerged, each with a comment where the session closed it: #6 (same flutter_local_notifications 18 to 22 break). The `native` check on #7 failed once on the reboot-reminder step with exit code 124 (timeout while the runners were saturated) and passed on the single re-run, so #7 merged. Live result when this entry was written: main CI green, branch protection and required contexts unchanged and each context produced by the last merged PR's check runs, private vulnerability reporting on, dependency graph on (SBOM 200). Dependabot, code-scanning and secret-scanning alerts and the repository security settings are read with the owner's token, not by the session; their state is recorded after the owner's next read.
- Alerts and repository settings read 2026-10-27 with the owner's token (a new token for the account that owns the repos; the session's proxy cannot read them): open Dependabot, code-scanning and secret-scanning alerts 0 before the changes below; fork-PR workflow approval `all_external_contributors`; private vulnerability reporting on. Secret scanning, push protection and Dependabot security updates were set by the owner (PATCH 200, automated-security-fixes 204) and read back as `enabled`; the secret-scanning read lists no open alert. The owner posted `@dependabot recreate` on #11 (HTTP 201). Dependabot recreated it, but it still carries flutter_local_notifications 18 to 22: the `update-types` ignore did not apply to that package, so `.github/dependabot.yml` now ignores it by version range (`>= 19.0.0`). Dependabot then recreated it as #14 with the same bump, and #14 was closed too (see Deferred).

## Time-limited exemptions

- None.

## Notes

- Merge policy (deliberate choice by the repo owner, the same blanket policy as the application repos): every PR, major-version dependency bumps included, is squash-merged as soon as all of its required checks are green, confirmed per PR. This repo is a code showcase with no business or sensitive dependency, so green checks are the only gate. Red, pending or conflicted PRs are fixed or closed instead.
- `main` protection (2026-10-10, read back): pull request required (0 approvals), enforced for admins, force pushes and deletions blocked, "require branches to be up to date" off (`strict` false) on purpose. Nobody pushes to `main` directly; status-log updates go through a PR too.
- Required contexts: `analyze-and-test`, `Secret scanning (gitleaks)`. Nothing else is required on purpose: osv-scanner, npm audit, dependency review and CodeQL depend on a moving advisory database or run only on some events.
- Closed unmerged on 2026-10-10, each with an explanatory comment: #1 and #2 (both carried `flutter_local_notifications` 20/22, a major that needs code changes). If Dependabot recreates one, apply the merge policy; the closure is not re-raised. Dependabot does not re-propose a major after its PR is closed, so a later upgrade is a manual change.
- The `native` check (`android-native.yml`) is path-filtered and only runs on `pull_request` or a manual dispatch, so it is deliberately not required: a docs-only PR would wait for it forever.
- `flutter_local_notifications` major bumps are ignored in `.github/dependabot.yml`: 18.x to 22.x renames the plugin API to named parameters (`flutter analyze` fails in `reminder_scheduler.dart`) and changes the native build. Migrate it as its own change, then drop the rule.

## Deferred (not re-raised each pass)

- Alert reads (Dependabot, code scanning, secret scanning) and Actions settings (fork-PR approval) are blocked for Claude sessions by the proxy; they are read and written with the repo owner's token on their own machine.
- The owner keeps the token's Administration, Contents and Secret scanning permissions and does not rotate it (deliberate, 2026-10-09, reconfirmed 2026-10-10 for the template repos).
- Dependabot commands (`@dependabot rebase`, `@dependabot recreate`) written by a Claude session are neutralised when the comment is posted (the mention is mangled, so Dependabot never sees it). Rebases and recreates of conflicted or stale PRs go through the owner's token; the session can only refresh a branch with the update-branch call or replace the PR with its own.
- flutter_local_notifications 18 to 22 was migrated on 2026-10-27: `reminder_scheduler.dart` uses the named-parameter API (`initialize(settings:)`, `zonedSchedule(id:, title:, body:, scheduledDate:, notificationDetails:, androidScheduleMode:)`, `cancel(id:)`) and the `uiLocalNotificationDateInterpretation` argument is gone. `flutter analyze` is clean and `flutter test` passes (21 tests) on Flutter 3.47.8; the Android native build and emulator run are checked by CI only (the sandbox cannot reach Google's Maven). The temporary ignore rule in `.github/dependabot.yml` was removed with it.
