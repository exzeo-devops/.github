# Terraform CI GitHub Action

This GitHub Action automates the process of running security checks, best practices, and unit tests for Terraform code. It ensures that security vulnerabilities are detected, code is formatted correctly, and infrastructure configurations are tested.

## Workflow Overview

The workflow consists of three main jobs:

- **Runs security checks**: Uses `tfsec` to scan the Terraform code for security issues and creates a report.
- **Runs best practices**: Ensures Terraform code is properly formatted according to best practices.
- **Runs unit tests**: Runs Terraform unit tests to verify the configurations.

## Workflow Triggers

This action is triggered on:

- Pull requests to the default branch (`pull_request` event).
- Pushes to the default branch (`push` event).

## Jobs

### Runs Security Checks Job

The `security` job performs the following actions:

- **Checkout Code**: Uses the `actions/checkout` action to fetch the repository code.
- **Install Terraform**: Uses the `hashicorp/setup-terraform` action to set up Terraform.
- **Get Token**: Uses the `actions/create-github-app-token` action to create a GitHub App token for authentication.
- **Run Terraform Security Checks using tfsec**: Uses the `aquasecurity/tfsec-action` to scan the Terraform code for security vulnerabilities.
- **Read Report**: Uses the `exzeo-devops/read-file-action` to read the generated security report.
- **Create PR Comment**: Uses the `marocchino/sticky-pull-request-comment` action to comment the security report on the pull request.

### Runs Best Practices Job

The `lint` job performs the following actions:

- **Checkout Repo**: Uses the `actions/checkout` action to fetch the repository code.
- **Install Terraform**: Uses the `hashicorp/setup-terraform` action to set up Terraform.
- **Check Terraform Format**: Ensures Terraform files are properly formatted by running `terraform fmt`.

### Runs Unit Tests Job

The `test` job performs the following actions:

- **Checkout Code**: Uses the `actions/checkout` action to fetch the repository code.
- **Install Terraform**: Uses the `hashicorp/setup-terraform` action to set up Terraform.
- **Assume Role**: Uses the `aws-actions/configure-aws-credentials` action to assume an AWS role for testing.
- **Initialize Terraform**: Runs `terraform init` to initialize the configuration.
- **Run Unit Tests**: Runs Terraform unit tests.

## Secrets

This action requires the following secrets:

- `GIT_AUTOMATION_APP_ID`: The GitHub App ID used for authentication.
- `GIT_AUTOMATION_PK`: The private key for the GitHub App used for authentication.
- `AWS_OIDC_AUDIENCE`: The audience for AWS OIDC authentication.
- `AWS_ROLE_ARN`: The ARN of the AWS role to assume.
- `AWS_REGION`: The AWS region to use.

This `README.md` provides an overview of the Terraform CI workflow to automate security checks, best practices, and testing of Terraform configurations in your repository.
