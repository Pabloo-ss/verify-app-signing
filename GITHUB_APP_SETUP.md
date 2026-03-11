# GitHub App Setup Instructions

## Creating the GitHub App

1. **Navigate to GitHub App creation page**
   - Go to: https://github.com/settings/apps/new
   - OR: Settings → Developer settings → GitHub Apps → New GitHub App

2. **Fill in the form**

   ### GitHub App name
   - **Value:** `Commit Signing Test` (or any unique name)
   - Must be globally unique across all GitHub Apps

   ### Homepage URL
   - **Value:** `https://github.com/YOUR-USERNAME/YOUR-REPO-NAME`
   - Replace with your actual test repo URL

   ### Webhook
   - **Active:** ❌ **UNCHECK** this box
   - We don't need webhooks for this test
   - **Webhook URL:** Leave blank (will be disabled)

   ### Permissions

   **Repository permissions:**
   - **Contents:** `Read and write` ✅
   - **Leave all other permissions at "No access"**

   **Account permissions:**
   - Leave all at "No access"

   ### Where can this GitHub App be installed?
   - **Select:** ✅ Only on this account

3. **Create the app**
   - Click "Create GitHub App" button
   - You'll be redirected to your new app's settings page

## After Creation

### Save the App ID
1. At the top of the app settings page, you'll see:
   - `App ID: 123456`
2. **Copy this number** - you'll add it as a secret named `APP_ID`

### Generate a Private Key
1. Scroll down to "Private keys" section
2. Click "Generate a private key"
3. A `.pem` file will download automatically
4. Open this file in a text editor
5. **Copy the entire contents** including the `-----BEGIN RSA PRIVATE KEY-----` and `-----END RSA PRIVATE KEY-----` lines
6. You'll add this as a secret named `APP_PRIVATE_KEY`

### Install the App on Your Test Repository
1. Scroll to the top of the app settings page
2. Click "Install App" in the left sidebar
3. Click "Install" next to your account name
4. **Select:** Only select repositories
5. **Choose:** Your test repository (the one you just created with the workflow)
6. Click "Install"

## Add Secrets to Your Repository

1. Go to your test repository on GitHub
2. Settings → Secrets and variables → Actions
3. Click "New repository secret"

### Add APP_ID
- **Name:** `APP_ID`
- **Value:** The numeric App ID you saved earlier (e.g., `123456`)
- Click "Add secret"

### Add APP_PRIVATE_KEY
- **Name:** `APP_PRIVATE_KEY`
- **Value:** Paste the entire contents of the `.pem` file, including the BEGIN and END lines
- Click "Add secret"

## Verification Checklist

Before running the workflow, verify:

- [ ] GitHub App created
- [ ] App has "Contents: Read and write" permission
- [ ] Private key generated and downloaded
- [ ] App installed on your test repository
- [ ] `APP_ID` secret added to repository
- [ ] `APP_PRIVATE_KEY` secret added to repository (entire PEM file contents)

## Running the Test

1. Push the workflow file to your test repository
2. Go to the "Actions" tab in your repository
3. Select "Test GPG-Signed Commit via GitHub API" workflow
4. Click "Run workflow" → "Run workflow"
5. Wait for the workflow to complete
6. The workflow output will show a commit URL
7. Visit that URL and look for the "Verified" badge

## Expected Result

The commit should show:
- ✅ **Verified** badge
- Signed by: `YOUR-APP-NAME[bot]`
- Message: "Update timestamp via GitHub App API"

## Troubleshooting

### "Failed to get installations"
- Check that your App ID is correct
- Verify the private key is the complete PEM file contents

### "Could not find installation ID"
- Make sure you installed the app on your test repository
- Check the app has access to the repository

### "Failed to get installation token"
- Verify the App ID matches the private key
- Check that the JWT was generated correctly

### Commit created but not verified
- This would mean the hypothesis is incorrect
- GitHub may not auto-sign commits from App Installation Tokens
- Document this finding for your Jenkins implementation
