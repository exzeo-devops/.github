# Build Docker Image GitHub Action

This GitHub Action automates the process of building and pushing Docker images. It is triggered on pull requests to the default branch and when tags are pushed, ensuring that the Docker image is always up-to-date.

## Workflow Overview

The workflow consists of a single job:

- **Build and push Docker image**: Checks out the code, builds the Docker image, and pushes it to the container registry.

## Workflow Triggers

This action is triggered on:

- Pull requests to the default branch (`pull_request` event).
- When tags are pushed (`push` event with tags matching `v*`).

## Jobs

### Build and Push Docker Image Job

The `build` job performs the following actions:

- **Checkout Repo**: Uses the `actions/checkout` action to fetch the repository code.
- **Get Token**: Uses the `actions/create-github-app-token` action to create a GitHub App token for authentication.
- **Wait for Workflows to Finish**: Uses the `int128/wait-for-workflows-action` action to wait for any running workflows to complete before continuing.
- **Installs QEMU Static Binaries**: Uses the `docker/setup-qemu-action` to enable cross-platform builds.
- **Set up Docker Buildx**: Uses the `docker/setup-buildx-action` to set up Docker Buildx for building multi-platform images.
- **Log in to Container Registry**: Uses the `docker/login-action` to authenticate with the container registry.
- **Extract Docker Metadata**: Uses the `docker/metadata-action` to extract metadata for the Docker image, including tags and labels.
- **Add SSH Key**: Uses the `webfactory/ssh-agent` action to add an SSH key for accessing private repositories.
- **Setup SSH Access for Private Repository**: Configures Git to use SSH for accessing GitHub repositories.
- **Install Dependencies (if Makefile exists and has init target)**: Runs the `make init` command if a Makefile with the `init` target exists.
- **Build and Push Docker Image**: Uses the `docker/build-push-action` to build the Docker image and push it to the container registry.

## Secrets

This action requires the following secrets:

- `GIT_AUTOMATION_APP_ID`: The GitHub App ID used for authentication.
- `GIT_AUTOMATION_PK`: The private key for the GitHub App used for authentication.
- `SSH_PRIVATE_KEY`: The private key used to authenticate with remote servers.

This `README.md` provides an overview of the Build Docker Image workflow to automate building and pushing Docker images in your repository.
