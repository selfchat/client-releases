# Working on selfchat

These rules apply to every selfchat repository: **selfchat/client** (the
app), **selfchat/server**, **selfchat/client-releases** (where the app is
built and published) and **selfchat/server-releases** (where selfchat-server
and its Docker image are built and published).

## Picking up work

The roadmap is GitHub milestones called phases (Phase 3: Calls, Phase 4:
Everyday messaging, and so on), with the same names in selfchat/client and
selfchat/server. Each issue sits in the phase that needs it.

- **Assign yourself before you start.** That's how others know it's taken.
  If someone is already assigned, ask them first.
- **Open a draft pull request early**, with `Closes #12` in the description.
  It shows the work is under way and links it to the issue. Mark it ready
  for review when it's done.
- **Stopping?** Unassign yourself and leave a comment on the issue saying
  where you got to, so someone else can carry on.
- **No issue yet?** Open one in the right phase first, unless it's a quick
  fix.
- To see who's doing what, open the phase's milestone: each open issue
  shows who it's assigned to.

## Branches

- `main` is always releasable. Nothing is pushed to it directly; every
  change goes through a pull request. Releases are the only exception
  (`pnpm release` commits the version bump and tags it).
- Branch from an up-to-date `main`. Name branches after the change, in
  lowercase with hyphens: `personal-node`, `fix-api-types-publish`,
  `status-menu`.
- Keep branches short-lived, merged within days. A branch can hold several
  related changes: one commit per change, in one pull request. Each commit
  should leave `main` working.
- **Branches are deleted as soon as they're merged.** GitHub does this
  automatically. Delete your local copy too:
  `git checkout main && git pull && git branch -d <branch>`.
- **Abandoned branches are removed weekly.** A scheduled job
  (`.github/workflows/stale-branches.yml`) deletes any branch with no commits
  for 30 days and no open pull request. To keep one, open a draft pull
  request for it.
- Tags are permanent: never delete or move a pushed tag.

## Pull requests

- Merges are **squash merges only**, so `main` gets one commit per pull
  request. The commit uses the pull request's title and description, so
  write both for someone reading `git log` later.
- Title: what changes, in plain words ("Short friend links", "Fix
  publishing @selfchat/server-api").
- Description: what changed and why, how it was tested, and what's left
  out. Close issues with `Closes #12`, one per line.
- Merge only when CI passes. If a check fails, fix it on the branch;
  don't merge around it.
- Work that needs the other repository: protocol changes land in
  selfchat/server first (see below). Link the pull requests to each other.

## Protocol changes

`selfchat-proto` lives in selfchat/server and the app pins it by tag.

1. Change `selfchat-proto` in a selfchat/server pull request and bump the
   workspace version (`0.4.0` → `0.5.0` for a breaking change).
2. Merge it, then tag `main`: `git tag -a v0.5.0 -m "selfchat-proto v0.5.0"`.
3. In selfchat/client, point `selfchat-proto` at the new tag in a pull request.

While both sides change together, the app may use
`{ path = "../selfchat-server/crates/selfchat-proto" }` locally, but never
merge a path dependency.

## Releases

- Desktop app: **merging a pull request into `main` in selfchat/client
  releases it.** The version goes up one patch (label the pull request
  `release:minor` or `release:major` for more, or `no-release` to skip),
  `main` is tagged, and the build in selfchat/client-releases publishes the
  installers, the signed update feed and the release notes.
- Release notes come from the pull request's `## Release notes` section:
  short bullets, in plain words, for people who use the app. Without the
  section, the pull request's title is used.
- By hand, without a pull request: `pnpm release X.Y.Z` from an
  up-to-date `main`.
- Test the build pipeline without publishing:
  `gh workflow run release.yml -R selfchat/client-releases -f tag=<branch> -f publish=false`.
- Never install or uninstall selfchat builds on a machine where someone
  uses selfchat for real. Installs share one folder, so a test install
  replaces theirs. Test on CI or a spare machine.

## Writing

- Plain words, in the app and in commits, pull requests, issues and
  release notes.
- Don't name other products. Describe the behaviour instead
  ("downloads in the background; click Restart to update").
- Keep secrets out of repositories: signing keys and deploy keys live in
  GitHub secrets.
