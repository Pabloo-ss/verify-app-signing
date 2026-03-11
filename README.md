# GitHub App Signed Commit Test

This repository tests whether GitHub App Installation Tokens produce GPG-signed commits when using the GitHub API directly.

## What This Tests

- JWT generation from GitHub App private key
- Exchange JWT for Installation Token
- Create commits via GitHub API (no git CLI)
- Verify commits show "Verified" badge on GitHub

## Setup Required

See the workflow file `.github/workflows/test-signed-commit.yml` for implementation details.

### Required Secrets

1. `APP_ID` - Your GitHub App's numeric ID
2. `APP_PRIVATE_KEY` - Your GitHub App's private key (PEM format)

## Running the Test

1. Go to Actions tab
2. Select "Test GPG-Signed Commit via GitHub API" workflow
3. Click "Run workflow"
4. Check the commit URL in the workflow output
5. Verify the commit has a "Verified" badge

## Expected Result

Commits created by GitHub Apps via the API should automatically show as "Verified" with the GitHub App's name as the signer.
