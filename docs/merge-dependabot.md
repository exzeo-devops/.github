# Auto-Merge Dependabot GitHub Action

This GitHub Action automates the process of merging Dependabot pull requests when they meet certain conditions. It ensures that Dependabot version updates are merged automatically for semver patch or minor updates.

## Workflow Overview

This workflow only runs on pull requests created by the Dependabot actor. The workflow consists of a single job:

- **Auto-merge dependabot**: Checks out the repository, waits for any running workflows to finish, and approves and merges the Dependabot pull request if it meets the specified conditions.

## Workflow Triggers

This action is triggered on:

- Pull requests to the default branch (`pull_request` event).

## Jobs

### Auto-Merge Dependabot Job

The `dependabot` job only runs if the actor is `dependabot[bot]`. It performs the following actions:

- **Checkout Repository**: Uses the `actions/checkout` action to fetch the repository code.
- **Get Token**: Uses the `actions/create-github-app-token` action to create a GitHub App token for authentication.
- **Wait for Workflows to Finish**: Uses the `int128/wait-for-workflows-action` action to wait for any running workflows to complete before continuing.
- **Dependabot Metadata**: Uses the `dependabot/fetch-metadata` action to fetch metadata about the Dependabot pull request.
- **Approve and Merge the Pull Request**: Uses the `actions/github-script` action to approve and merge the pull request if it is a semver patch or minor update.

## Secrets

This action requires the following secrets:

- `GIT_AUTOMATION_APP_ID`: The GitHub App ID used for authentication.
- `GIT_AUTOMATION_PK`: The private key for the GitHub App used for authentication.

This `README.md` provides an overview of the Auto-Merge Dependabot workflow to automate merging of Dependabot pull requests in your repository.
