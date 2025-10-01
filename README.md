# GitHub Collaborator Management Workflow

This workflow provides automated management of GitHub repository collaborators using composite actions.

## Structure

```text
.github/
├── workflows/
│   └── manage-collaborators.yml
└── actions/
├── list-collaborators/
│   └── action.yml
├── add-collaborator/
│   └── action.yml
└── remove-collaborator/
└── action.yml
```
## Features

- **List Collaborators**: View all collaborators for a repository
- **Add Collaborator**: Invite or update collaborator permissions
- **Remove Collaborator**: Remove a collaborator from a repository

## Setup

1. Create a GitHub Personal Access Token (PAT) or GitHub App with the following permissions:
   - `repo` (full control of private repositories)
   - `admin:repo_hook` (for collaborator management)

2. Add the token as a secret in your repository:
   - Go to Settings → Secrets and variables → Actions
   - Create a new secret named `GITHUB_TOKEN_GRAINED`
   - Paste your token value

3. Copy all files to your repository maintaining the directory structure

## Usage

1. Go to the **Actions** tab in your repository
2. Select **TPT-GEN-CollaboratorsIACActions** workflow
3. Click **Run workflow**
4. Fill in the required parameters:
   - **Action**: Choose the operation (invite/remove/list)
   - **Repository**: Target repository in `owner/repo` format
   - **Collaborator**: Username (required for invite/remove)
   - **Permission**: Access level (required for invite)

## Permission Levels

- `pull`: Read-only access
- `triage`: Read access + manage issues/PRs
- `push`: Read/write access
- `maintain`: Push access + manage repository settings (without destructive actions)
- `admin`: Full administrative access

## Composite Actions

Each operation is implemented as a reusable composite action:

### list-collaborators
Lists all collaborators with their roles and permissions.

**Inputs:**
- `repository`: Target repository (required)

### add-collaborator
Adds or updates a collaborator's permissions.

**Inputs:**
- `repository`: Target repository (required)
- `collaborator`: GitHub username (required)
- `permission`: Permission level (required)

### remove-collaborator
Removes a collaborator from the repository.

**Inputs:**
- `repository`: Target repository (required)
- `collaborator`: GitHub username (required)

## Security

- Secrets are passed via environment variables at the job level
- Token is never exposed in logs or action inputs
- All API calls use the latest GitHub API version (2022-11-28)

## Troubleshooting

### 404 Not Found
- Verify repository name format (`owner/repo`)
- Ensure token has access to the repository
- Check that the collaborator username is correct

### 403 Forbidden
- Verify token has admin permissions on the repository
- Check organization security policies
- Ensure API rate limits haven't been exceeded

### 422 Unprocessable Entity
- Verify permission level is valid
- Check collaborator username format

## License

MIT
