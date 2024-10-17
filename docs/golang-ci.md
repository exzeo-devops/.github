# Golang CI GitHub Action

This GitHub Action automates the process of linting, testing, and securing Go code. It ensures that linting, unit tests, CodeQL analysis, and security scans are performed on each pull request and push to the default branch.

## Workflow Overview

The workflow consists of four main jobs:

- **Run golang linting**: Checks for linting issues in the Go code.
- **Run golang unit tests**: Runs unit tests and uploads the test results.
- **Scan code using CodeQL**: Performs static code analysis using GitHub CodeQL.
- **Scan code using golang security tools**: Scans the Go code for vulnerabilities and other security issues.

## Workflow Triggers

This action is triggered on:

- Pull requests to the default branch (`pull_request` event).
- Pushes to the default branch (`push` event).

## Jobs

### Run Golang Linting Job

The `lint` job performs the following actions:

- **Checkout Repo**: Uses the `actions/checkout` action to fetch the repository code.
- **Setup Go**: Uses the `actions/setup-go` action to set up the Go environment based on the version specified in `go.mod`.
- **Check go.mod and go.sum Tidiness**: Runs `go mod tidy` and checks if the `go.mod` or `go.sum` files need updating.

### Run Golang Unit Tests Job

The `tests` job performs the following actions:

- **Checkout Repo**: Uses the `actions/checkout` action to fetch the repository code.
- **Setup Go**: Uses the `actions/setup-go` action to set up the Go environment based on the version specified in `go.mod`.
- **Add SSH Key**: Uses the `webfactory/ssh-agent` action to add an SSH key for accessing private repositories.
- **Setup SSH Access for Private Repository**: Configures Git to use SSH for accessing GitHub repositories.
- **Install Dependencies**: Runs `go get `. to install trequired dependencies.
- **Run Unit Tests**: Runs `go test` and saves the results in JSON format.
- **Upload Go Test Results(**: Uses the `actions/upload-artifact` action to upload the test results as an artifact.

### Scan Code Using CodeQL Job

The `codeql` job performs the following actions:

- **Checkout Repo**: Uses the `actions/checkout` action to fetch the repository code.
- **Get Token**: Uses the `actions/create-github-app-token` action to create a GitHub App token for authentication.
- **Initialize CodeQL**: Uses the `github/codeql-action/init` action to initialize CodeQL for Go.
- **Perform CodeQL Analysis**: Uses the `github/codeql-action/analyze` action to perform static analysis of the code.

### Scan Code Using Golang Security Tools Job

The `security` job performs the following actions:

- **Checkout Repo**: Uses the `actions/checkout` action to fetch the repository code.
- **Get Token**: Uses the `actions/create-github-app-token` action to create a GitHub App token for authentication.
- **Setup Go**: Uses the `actions/setup-go` action to set up the Go environment based on the version specified in `go.mod`.
- **Run Gitleaks**: Installs and runs Gitleaks to scan for hard-coded secrets.
- **Run GoSec**: Installs and runs GoSec to identify security vulnerabilities in Go code.
- **Run Govulncheck**: Installs and runs Govulncheck to check for known vulnerabilities in the Go code.
- **Upload Security Reports(**: Uses the `github/codeql-action/u
