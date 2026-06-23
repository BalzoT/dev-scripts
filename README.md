# dev-scripts

Personal developer workflow scripts for recurring operations.

This repository contains small executable helpers that automate repeatable development tasks. Some scripts are Git-specific, while others may cover general local workflow operations. They are intentionally lightweight Bash commands that can be run from anywhere once this directory is on your `PATH`.

## Scripts

Current scripts focus on Git workflows.

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

Make sure the scripts are executable.

```sh
chmod +x checkout-ticket discard-changes
```

### Add the scripts to your global `PATH`

Add this repository directory to your shell `PATH` so the scripts can be run from any terminal session and from inside any project.

For `zsh`, add this line to `~/.zshrc`:

```sh
export PATH="$PATH:/Users/{YOUR_PATH}/Code/dev-scripts"
```

Then reload your shell configuration:

```sh
source ~/.zshrc
```

For `bash`, add the same export line to `~/.bashrc` or `~/.bash_profile`, then reload that file.

Verify that the commands are globally available:

```sh
command -v checkout-ticket
command -v discard-changes
```

After that, you can run the scripts by name from any Git repository:

```sh
checkout-ticket cb1-246
discard-changes
```

## Requirements

- Bash
- Git, for scripts that operate on Git repositories
- A repository that uses `develop` as the integration branch for `checkout-ticket`
