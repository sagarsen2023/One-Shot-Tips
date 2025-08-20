# GitHub Actions NPM Publishing Setup

## Setup the github workflow file in `.github/workflows/.publish-to-npm.yml` file
```yml
name: Publish Package to npmjs
on:
  push:
    branches:
      - release

  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write
    steps:
      - uses: actions/checkout@v4
      # Setup .npmrc file to publish to npm
      - uses: actions/setup-node@v4
        with:
          node-version: "20.x"
          registry-url: "https://registry.npmjs.org"
      - run: npm ci
      - run: npm run build
      - run: npm publish --provenance --access public
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```


## Required: NPM Token Setup 🔑

To publish packages to npm, you need to set up an NPM_TOKEN secret in your GitHub repository:

### Step 1: Get Your NPM Token
1. Go to [npmjs.com](https://www.npmjs.com/) and log in
2. Click on your profile picture → "Access Tokens"
3. Click "Generate New Token" → "Classic Token"
4. Select "Automation" (recommended for CI/CD)
5. Copy the generated token

### Step 2: Add Token to GitHub Secrets
1. Go to your GitHub repository: `https://github.com/<your_username>/<repository_name>`
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Click "New repository secret"
4. Name: `NPM_TOKEN`
5. Value: Paste your npm token
6. Click "Add secret"

## Workflow Features 🛠️

- ✅ **Automatic Building**: Runs `npm run build` before publishing
- ✅ **Provenance**: Includes provenance statements for supply chain security
- ✅ **Public Access**: Publishes as a public package
- ✅ **Node 20.x**: Uses the latest LTS Node.js version
- ✅ **Manual Trigger**: Can be triggered manually from GitHub Actions tab

## Monitoring 📊

After pushing to the release branch:
1. Go to the **Actions** tab in your GitHub repository
2. Look for the "Publish Package to npmjs" workflow
3. Monitor the progress and check for any errors

## Troubleshooting 🔧

### Common Issues:
1. **NPM_TOKEN not set**: Add the token as described above
2. **Version already exists**: Make sure to increment the version number
3. **Build fails**: Ensure all tests pass and dependencies are installed
4. **Permission denied**: Check that your npm token has publish permissions

### Logs:
- Check GitHub Actions logs for detailed error messages
- Verify npm package page for successful publications

## Security Notes 🔒

- Never commit npm tokens to your repository
- Use "Automation" tokens for CI/CD (more secure than "Publish" tokens)
- Tokens are stored securely in GitHub Secrets
- Provenance statements help verify package authenticity
