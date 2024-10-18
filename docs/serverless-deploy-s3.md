# Deploy Serverless via S3 GitHub Action

This GitHub Action automates the deployment of serverless applications via S3. It builds, packages, and uploads serverless code to an S3 bucket, and then updates the Lambda functions with the new code.

## Workflow Overview

The workflow consists of two main jobs:

- **Setup Deployment Matrix**: Reads the deployment environments from a `settings.yaml` file and outputs a matrix configuration for deployment.
- **Build and Upload to S3**: Packages the serverless application, uploads it to S3, and updates the corresponding Lambda function.

## Workflow Triggers

This action is triggered on:

- Manual triggers (`workflow_dispatch` event).
- Pushes to tags matching the pattern `v*` (`push` event).

## Jobs

### Setup Deployment Matrix Job

The `setup` job performs the following actions:

- **Checkout Repo**: Uses the `actions/checkout` action to fetch the repository code.
- **Set up Node.js**: Uses the `actions/setup-node` action to set up Node.js for running scripts.
- **Install Dependencies**: Installs the required npm packages.
- **Read Matrix from settings.yaml**: Reads the deployment environments from `.github/settings.yaml` and outputs a matrix for the next job.

### Build and Upload to S3 Job

The `upload` job performs the following actions:

- **Checkout Repository**: Uses the `actions/checkout` action to fetch the repository code.
- **Get Token**: Uses the `actions/create-github-app-token` action to create a GitHub App token for authentication.
- **Wait for Workflows to Finish**: Uses the `int128/wait-for-workflows-action` action to wait for any running workflows to complete before continuing.
- **Assume Role**: Uses the `aws-actions/configure-aws-credentials` action to assume an AWS role for deployment.
- **Check if S3 Bucket Exists**: Checks if the specified S3 bucket exists and is accessible.
- **Package Code**: Runs the `make package` command to create a zipped executable.
- **Check for Package**: Ensures the package file exists after packaging.
- **Upload to S3**: Uploads the packaged file to the specified S3 bucket.
- **Update Lambda**: Updates the Lambda function with the new code uploaded to S3.

## Example settings.yaml

```
serverless:
  environments:
    - name: <account-name>
      region: <region>
```

## Secrets

This action requires the following secrets:

- `GIT_AUTOMATION_APP_ID`: The GitHub App ID used for authentication.
- `GIT_AUTOMATION_PK`: The private key for the GitHub App used for authentication.
- `AWS_OIDC_AUDIENCE`: The audience for AWS OIDC authentication.
- `AWS_ROLE_ARN`: The ARN of the AWS role to assume.
- `S3_BUCKET_NAME`: The name of the S3 bucket used for deployment.
