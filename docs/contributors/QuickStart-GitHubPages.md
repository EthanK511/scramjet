# Quick Start: Deploy to GitHub Pages

Get your Scramjet fork deployed to GitHub Pages in 3 simple steps!

## Step 1: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** → **Pages** (in the sidebar)
3. Under **Source**, select: **GitHub Actions**
4. Save

That's it! GitHub Pages is now enabled.

## Step 2: Enable Workflows

If you forked this repository, workflows might be disabled:

1. Go to the **Actions** tab in your repository
2. If you see a message about workflows being disabled, click **I understand my workflows, enable them**

## Step 3: Deploy

### Option A: Automatic Deployment (Recommended)

Simply merge your changes to the `main` branch:

```bash
git checkout main
git merge your-branch
git push origin main
```

The GitHub Actions workflow will automatically build and deploy your site!

### Option B: Manual Deployment

Trigger a deployment manually from any branch:

1. Go to **Actions** tab
2. Select **Deploy to GitHub Pages (Manual)**
3. Click **Run workflow**
4. Choose your branch (or leave as `main`)
5. Click **Run workflow**

## Step 4: Access Your Site

Your site will be available at:

```
https://<your-username>.github.io/scramjet/
```

Replace `<your-username>` with your GitHub username.

## First Deployment

⏱️ The first deployment takes about 5-10 minutes because it needs to:
- Build the Rust rewriter (WASM)
- Compile TypeScript
- Generate TypeDoc documentation
- Bundle everything together

Subsequent deployments are faster (2-3 minutes) thanks to caching!

## Troubleshooting

### Workflow not running?
- Make sure you're on the `main` branch or use the manual workflow
- Check that Actions are enabled (Settings → Actions → General)

### Build failed?
- Check the Actions tab for error details
- Common issue: Rust/WASM tools may need to build from scratch on first run

### Site not loading?
- Wait a few minutes after the first deployment
- Check that GitHub Pages is set to "GitHub Actions" source
- Clear your browser cache

## Need Help?

- Read the [full deployment guide](GitHubPages.md)
- Check the [Actions logs](../../actions) for error details
- Review the [main workflow file](../../.github/workflows/main.yml)

---

🎉 **That's it!** Your Scramjet demo is now live on GitHub Pages!
