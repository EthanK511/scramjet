# GitHub Pages Deployment Guide

This guide explains how to deploy Scramjet to GitHub Pages for your fork.

## Prerequisites

- A fork of the Scramjet repository
- GitHub Pages enabled in your repository settings
- Workflows enabled in your fork

## Automatic Deployment

The repository includes a GitHub Actions workflow (`.github/workflows/main.yml`) that automatically deploys to GitHub Pages when code is pushed to the `main` branch.

### Manual Deployment

If you want to trigger a deployment manually (e.g., from a feature branch for testing), you can use the manual deployment workflow:

1. Go to the **Actions** tab in your repository
2. Select **Deploy to GitHub Pages (Manual)** from the workflows list
3. Click **Run workflow**
4. Optionally specify a branch (defaults to `main`)
5. Click **Run workflow** to start the deployment

This is useful for:
- Testing changes before merging to main
- Deploying from a specific branch
- Re-deploying without making new commits

### What Gets Deployed

The GitHub Pages site includes:
- The Scramjet demo application (from the `/static` directory)
- Built Scramjet bundles (from the `/dist` directory)
- TypeDoc documentation (both user-facing and developer-facing)
- Required dependencies (bare-mux, epoxy-transport, libcurl-transport)

### Deployment Process

When you push to the `main` branch, the workflow:

1. **Builds Scramjet**: Compiles the TypeScript/Rust code and bundles everything
2. **Runs Tests**: Validates the build with package validation and integration tests
3. **Builds TypeDoc**: Generates API documentation with version history
4. **Assembles Static Site**: Combines all assets into a `staticbuild` directory
5. **Deploys to GitHub Pages**: Uploads and publishes to GitHub Pages

## Enabling GitHub Pages in Your Fork

If you've forked this repository, you need to enable GitHub Pages:

1. Go to your repository on GitHub
2. Click on **Settings**
3. Scroll down to **Pages** in the left sidebar
4. Under **Source**, select:
   - Source: **GitHub Actions**
5. Save the settings

## Permissions

The workflow requires specific permissions to deploy to GitHub Pages. These are already configured in the workflow file:

```yaml
permissions: write-all
```

If deployment fails with permission errors, ensure:
- Actions have read/write permissions in your repository settings
- GitHub Pages is enabled and set to use GitHub Actions as the source

## Configuration

The deployment uses default configuration specified in `ci/build-static.sh`:

```javascript
{
  "wispurl": "wss://anura.pro/",
  "bareurl": "https://aluu.xyz/bare/"
}
```

To customize these URLs for your deployment, you can:

1. Fork the repository
2. Modify the `ci/build-static.sh` script to use your own WISP and Bare server URLs
3. Push to the `main` branch

## Manual Deployment (for testing)

To test the deployment locally before pushing:

```bash
# Install dependencies
pnpm install

# Build everything
pnpm build

# Build the TypeDoc documentation
pnpm run docs
pnpm run docs:dev

# Build the static site
bash ./ci/build-static.sh

# Serve the static site locally (optional)
cd staticbuild
python3 -m http.server 8080
```

Then open http://localhost:8080 in your browser to preview the site.

## Troubleshooting

### Workflow Not Running

**Problem**: The workflow doesn't run when you push code.

**Solutions**:
- Ensure you're pushing to the `main` branch
- Check that GitHub Actions is enabled in your repository settings
- Verify that the workflow file exists at `.github/workflows/main.yml`

### Deployment Fails with Permission Errors

**Problem**: The deployment step fails with a permissions error.

**Solutions**:
- Go to Settings → Actions → General
- Under "Workflow permissions", select "Read and write permissions"
- Enable "Allow GitHub Actions to create and approve pull requests"

### GitHub Pages Not Available

**Problem**: The deployed site doesn't load or shows 404.

**Solutions**:
- Ensure GitHub Pages is enabled in repository settings
- Verify the source is set to "GitHub Actions"
- Wait a few minutes after the first deployment (it can take time to provision)
- Check the Actions tab for any deployment errors

### Build Fails

**Problem**: The build step fails in the workflow.

**Solutions**:
- Check the specific error in the Actions logs
- Ensure all dependencies are correctly specified in `package.json`
- Verify that Rust toolchain and WASM tools are properly configured in the workflow
- Try building locally to reproduce the issue

## Accessing Your Deployed Site

Once deployed, your site will be available at:

```
https://<your-username>.github.io/scramjet/
```

For example, if your username is `john`, the URL would be:
```
https://john.github.io/scramjet/
```

If you have a custom domain configured for your GitHub Pages, it will use that domain instead.

## Additional Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Scramjet Official Demo](https://scramjet.mercurywork.shop/)
