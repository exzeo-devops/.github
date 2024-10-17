
# Auto Rebase PR GitHub Action

This GitHub Action automates the process of rebasing pull requests when a specific command is added as a comment on an issue. It listens for issue comments and, if the command `/rebase` is found, triggers an automatic rebase of the pull request.

## Workflow Overview

The workflow consists of a single job:

- **Auto-rebase PR**: Rebases the pull request when triggered by an issue comment.

## Workflow Triggers

This action is triggered on:

- Issue comments (`issue_comment` event) with the `created` type.

## Jobs

### Auto-rebase PR Job

The `auto-rebase` job performs the following actions:

- **Get Token**: Uses the `actions/create-github-app-token` action to create a GitHub App token for authentication.
- **Acknowledge the Rebase Request**: Uses the `actions/github-script` action to add a `+1` reaction to the comment that triggered the rebase.
- **Rebase the PR**: Uses the `actions/github-script` action to attempt to rebase the pull request. If the rebase fails, it comments on the pull request with the error message.

## Secrets

This action requires the following secrets:

- `GIT_AUTOMATION_APP_ID`: The GitHub App ID used for authentication.
- `GIT_AUTOMATION_PK`: The private key for the GitHub App used for authentication.

This `README.md` provides an overview of the Auto Rebase PR workflow to automate the rebasing of pull requests when triggered by a comment.
