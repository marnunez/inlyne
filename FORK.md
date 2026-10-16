# Maintaining this fork

`marnunez/inlyne` keeps the opt-in link-hints feature on `main`. The original
`feat/link-hints` branch remains as the submitted feature snapshot. `upstream`
points to `Inlyne-Project/inlyne`; use `origin/main` for this fork.

## Automatic upstream updates

[Sync upstream](https://github.com/marnunez/inlyne/actions/workflows/sync-upstream.yml)
checks upstream `main` daily at **07:23 UTC** (GitHub may delay scheduled runs).
You can also use **Actions → Sync upstream → Run workflow** on `main`.

- No new commits: scheduled runs skip compilation. Manual runs validate the current tree.
- New commits: merge into a temporary checkout and run workspace formatting, tests,
  strict Clippy and build checks on Ubuntu with the default X11/Wayland features.
- Success: push the exact tested merge commit, preserving our changes and history.
- Conflict/check failure: leave `main` unchanged and report the logs.
- Workflow files changed: report **manual merge needed** unless the optional token
  described below is configured. GitHub's built-in token cannot publish those files.
- If `main` moves during testing, refuse to publish; the next run retries from the new base.

The validation job has read-only repository access. A separate publish job verifies
and pushes the tested Git bundle without executing candidate code with write
permissions. It never force-pushes or publishes tags. The workflow is restricted
to this fork's `main`; normal source updates need no personal access token or
upstream permissions. These are Linux checks, not the inherited Windows/macOS or
typos jobs: pushes using `GITHUB_TOKEN` do not trigger the separate upstream CI.

The publish job also re-enables this workflow through GitHub's API, preventing the
60-day inactivity suspension during long upstream pauses without dummy commits.
GitHub Actions and hosted-runner availability are still required.

## Optional permission for workflow-file updates

For fully automatic updates that include `.github/workflows/`, create a fine-grained
personal access token scoped **only to `marnunez/inlyne`**, with **Contents: write**
and **Workflows: write**, and store it as the `UPSTREAM_SYNC_TOKEN` Actions secret.
Only the isolated publish checkout uses it. Validation and notifications continue
to use the built-in token. No such secret is configured by default.

Without it, the workflow validates the merge but does not publish it; the sync log
asks for a manual merge. After doing that locally, push `main` normally. Do not
force-sync or discard our feature to bypass the restriction.

## Notifications

Subscribe to [the sync log](https://github.com/marnunez/inlyne/issues/1) for GitHub/email
notifications. Updates and failures mention `@marnunez` there, with upstream commit,
comparison and run links. Identical scheduled results are not repeated; explicit
manual runs always report their result. Notification delivery follows your GitHub
notification settings.

The `UPSTREAM_SYNC_ISSUE` repository variable selects the log issue (currently `1`).
If replacing the log, update that variable. Keep Issues enabled and the log open.

## Local updates

Automation updates GitHub, not your local checkout or installed executable:

```sh
git switch main
git pull --ff-only origin main
```

Then rebuild using your normal development/package environment. This fork's added
automation does not change the upstream CI/release workflows or install anything
on your machine.
