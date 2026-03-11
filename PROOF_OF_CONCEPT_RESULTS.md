# Proof of Concept Results

## ✅ **SUCCESS! GitHub App Installation Tokens DO produce GPG-signed commits**

### Test Results

**Commit SHA:** `43c2609dab000ff3f5fa8c8eb473ef6a2e168eaa`
**Commit URL:** https://github.com/Pabloo-ss/verify-app-signing/commit/43c2609dab000ff3f5fa8c8eb473ef6a2e168eaa
**Verified:** ✅ **TRUE**
**Verification Reason:** `valid`
**Signed By:** `commitsigntest-1773234679[bot]`

### What This Proves

1. **No GPG tooling needed** - The commit was created using only the GitHub API
2. **No git CLI needed** - Pure `curl` + `jq` + `openssl` in bash
3. **Automatic signing** - GitHub automatically signs commits when using App Installation Tokens
4. **Verified badge** - The commit shows as "Verified" on GitHub with the app's name

### Setup Details

**GitHub App:**
- Name: `CommitSignTest-1773234679`
- App ID: `3065208`
- Permissions: `contents: write` (minimal required)
- Installed on: `Pabloo-ss/verify-app-signing`

**Authentication Flow:**
1. Generate JWT from App's private key using `openssl`
2. Exchange JWT for Installation Token via GitHub API
3. Use Installation Token to create commits via GitHub API
4. GitHub automatically signs commits with the App's identity

### Key Workflow Steps

The workflow in `.github/workflows/test-signed-commit.yml` demonstrates:

1. **JWT Generation** (using only `openssl`, `jq`, `tr`):
   ```bash
   # Create RS256 JWT with App ID
   # Sign with private key
   # Base64url encode
   ```

2. **Token Exchange**:
   ```bash
   # GET /app/installations (with JWT)
   # POST /app/installations/{id}/access_tokens
   ```

3. **Commit Creation** (pure GitHub API):
   ```bash
   # GET ref (get current commit SHA)
   # GET commit (get tree SHA)
   # POST git/trees (create new tree)
   # POST git/commits (create commit)
   # PATCH git/refs (update branch)
   ```

### Jenkins Implementation

This same flow can be replicated in Jenkins:
- Store App ID and private key in Jenkins credentials
- Use bash/curl/jq (available on most Jenkins agents)
- No need for git CLI or GPG tooling
- Commits will be automatically signed by GitHub

### Important Notes

- **Signature source**: Commits are signed by GitHub using the App's identity
- **Committer**: Shows as `appname[bot]` in commit history
- **Verification**: GitHub displays "Verified" badge automatically
- **No user GPG key needed**: The App itself is the signing identity

## Conclusion

✅ **Hypothesis confirmed**: GitHub App Installation Tokens produce GPG-signed commits when used with the GitHub API, with no need for GPG tooling or git CLI on the runner.

This approach is ready to be implemented in your Jenkins pipeline.
