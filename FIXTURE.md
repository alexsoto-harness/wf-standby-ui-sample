# wf-standby-ui-sample

This repository is a **comparison fixture**. It has the same Git and GitHub metadata shape as `wf-standby-migrate-sample`.

Import one with the Harness Code UI (**Import Repository**) and the other with `harness-migrate`. Differences you see in Harness Code come from the import tool, not from different source data.

## What is in this repo

| Object | Where to look on GitHub |
| --- | --- |
| Branches | `main`, `develop`, `release/0.1`, `feature/add-healthcheck`, `feature/readme-badge`, `feature/deprecated-flag` |
| Tags | annotated `v0.1.0`, annotated `v0.2.0`, lightweight `nightly-demo` |
| Pull requests | #1 open, #2 merged, #3 closed without merge |
| General PR comments | comment on each pull request |
| Line review comment | pull request #1, `src/health.js` |
| Labels | `migration-demo`, `failover`, `security`, plus default `bug` / `enhancement` |
| Branch protection | `main`: PR required, 1 approval, dismiss stale reviews, code owners, linear history, conversation resolution, no force-push, no deletion |
| CODEOWNERS | `* @alexsoto-harness` |
| Webhooks | `.../git-events` (push, create, delete) and `.../pull-request` (pull_request, review, review comment) |
| Issues | #1 open (milestone), #2 closed |
| Milestone | `Standby 0.2` |
| Release | `v0.1.0` |

## What each import path keeps

| Object | UI Import Repository | harness-migrate (default) |
| --- | --- | --- |
| Commits, branches, tags, files | Yes | Yes |
| Pull requests | No | Yes |
| PR comments and review comments | No | Yes, with gaps (reviewers/approvers, reactions, and attachments are not fully imported) |
| Labels | No | Yes |
| Branch protection | No | Yes, mapped subset |
| Webhooks | No | Yes, mapped subset. Disable these on the standby. |
| Issues | No | No |
| Milestones | No | No |
| Releases | No | No |

## Flags to show on `wf-standby-ui-sample`

Export flags (they drop data from the zip; import a **new** empty Harness repo each time):

- `--no-pr` — git arrives, pull requests do not
- `--no-comment` — pull requests arrive without comments
- `--no-webhook` — webhooks are left out of the export
- `--no-rule` — branch rules are left out of the export

Import flag:

- `--no-git` — for a Harness repo that **already** has the git history. It does **not** update pull requests in place. It inserts them again under new numbers (`highest existing PR number + source PR number`). The Harness repo is temporarily unavailable during that import.

Do the `--no-git` demo only after the first migrate import, and only with a **new** pull request created on GitHub after that import. Re-exporting these original three pull requests and importing with `--no-git` duplicates them under shifted numbers.

Suggested second-wave pull request (create it after the first import):

```bash
git checkout -b feature/post-import-pr
echo "created after the first import" >> POST_IMPORT.md
git add POST_IMPORT.md && git commit -m "Add post-import pull request fixture"
git push -u origin feature/post-import-pr
gh pr create --base main --title "Post-import PR for --no-git" --body "Created after the first migrate. Import with --no-git and compare the Harness PR number to this GitHub number."
```
