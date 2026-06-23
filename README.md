# dev-scripts

Personal developer workflow scripts for recurring Git tasks.

This repository contains small executable helpers that automate common branch and working-tree operations. The scripts are intentionally lightweight Bash commands that can be run from any Git repository once this directory is on your `PATH`.

## Scripts

### `checkout-ticket`

Finds and checks out a remote branch by ticket number.

```sh
checkout-ticket <ticket-number> [--rebase-develop]
```

Example:

```sh
checkout-ticket cb1-246 --rebase-develop
```

What it does:

- Requires a clean working tree before it starts.
- Checks out `develop` and pulls it with `git pull --ff-only`.
- Fetches and prunes remote branches.
- Searches `origin/*` for branches matching the ticket number, case-insensitively.
- Prompts when multiple matching branches are found.
- Checks out the selected branch, creating a local tracking branch if needed.
- Resets the local branch to `origin/<branch>`.
- Optionally attempts to rebase the branch onto `develop`; if conflicts occur, the rebase is aborted.

Because this script runs `git reset --hard origin/<branch>`, it is meant for workflows where the local ticket branch should match the remote branch exactly before review or further work.

### `discard-changes`

Discards tracked working-tree changes in the current Git repository.

```sh
discard-changes
```

What it does:

- Verifies it is running inside a Git repository.
- If there are tracked unstaged changes, runs `git checkout .`.
- Leaves staged changes and untracked files alone.

## Setup

Make sure the scripts are executable and add this repository to your shell `PATH`.

```sh
chmod +x checkout-ticket discard-changes
export PATH="$PATH:/path/to/dev-scripts"
```

To make the `PATH` change permanent, add the export line to your shell profile, such as `~/.zshrc`.

## Requirements

- Bash
- Git
- A repository that uses `develop` as the integration branch for `checkout-ticket`
