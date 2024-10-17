# Ansible CI GitHub Action

This GitHub Action automates the process of linting and testing Ansible configurations using Molecule. It ensures that linting, YAML checks, and Molecule tests are performed on each pull request and push to the default branch, maintaining the quality and functionality of your Ansible codebase.

## Workflow Overview

The workflow consists of three main jobs:

- **Lint Job**: Checks for linting issues in Ansible playbooks and YAML files.
- **Setup Job**: Reads the matrix configuration from a `settings.yaml` file and prepares the testing environment.
- **Test Job**: Runs Molecule tests based on the scenarios defined in the matrix.

## Workflow Triggers

This action is triggered on:

- Pull requests to the default branch (`pull_request` event).
- Pushes to the default branch (`push` event).

## Secrets

This action requires the following secret:

- `SSH_PRIVATE_KEY`: The private key used to authenticate with remote servers.

## Matrix Configuration

The matrix configuration for this workflow is loaded from the `.github/settings.yaml` file. Each scenario in the matrix includes:

- `name`: The name of the Ansible scenario.
- `image`: The Docker image used for Molecule tests.
- `playbook`: The Ansible playbook file to run.

Here is an example `settings.yaml` file:

```yaml
# Ansible playbook scenarios
ansible:
  scenarios:
    - name: default
      image: ubuntu-2404-ansible
      playbook: converge.yml
```

## Running Locally

To run the linting and testing locally:

1. Install dependencies with `pipenv install --dev`.
2. Run Ansible linting with `ansible-lint`.
3. Run Molecule tests for a scenario:
   ```bash
   pipenv run molecule test --scenario-name <scenario-name>
   ```

This `README.md` provides an overview of the Ansible CI workflow to maintain the quality of your Ansible roles and playbooks.
