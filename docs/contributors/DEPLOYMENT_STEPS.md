# GitHub Pages Deployment - Step by Step Instructions

This document provides detailed, visual instructions for deploying your Scramjet fork to GitHub Pages.

## Prerequisites ✅

Before you begin, make sure you have:
- [x] A fork of this repository
- [x] Admin access to your fork
- [x] Basic knowledge of Git and GitHub

---

## Part 1: Enable GitHub Pages (One-Time Setup)

### Step 1: Navigate to Repository Settings

1. Open your fork on GitHub: `https://github.com/<your-username>/scramjet`
2. Click the **⚙️ Settings** tab (top menu bar)

### Step 2: Find GitHub Pages Settings

1. In the left sidebar, scroll down to find **Pages**
2. Click on **Pages**

### Step 3: Configure GitHub Pages

1. Under **Build and deployment**:
   - **Source**: Select **GitHub Actions** from the dropdown
   - Leave all other settings as default
2. Click **Save** if there's a save button (may auto-save)

✅ **GitHub Pages is now enabled!**

---

## Part 2: Enable GitHub Actions (If Needed)

If you've forked this repository, workflows might be disabled by default.

### Step 1: Check Actions Status

1. Go to the **Actions** tab (top menu bar)
2. Look for any banner messages

### Step 2: Enable Workflows

If you see: *"Workflows aren't being run on this forked repository"*

1. Click the green button: **I understand my workflows, go ahead and enable them**

✅ **Actions are now enabled!**

---

## Part 3A: Automatic Deployment (Recommended for Production)

### Trigger Automatic Deployment

Any push to the `main` branch will automatically deploy to GitHub Pages.

```bash
# Make sure you're on main branch
git checkout main

# Pull latest changes (if any)
git pull origin main

# Merge your feature branch (replace 'your-branch' with your branch name)
git merge your-branch

# Push to trigger deployment
git push origin main
```

### Monitor Deployment

1. Go to the **Actions** tab
2. You should see a workflow run starting with name **"CI"**
3. Click on it to see progress
4. The workflow includes these jobs:
   - ✅ Build
   - ✅ Package Validation
   - ✅ Tests
   - ✅ **Pages** (this deploys to GitHub Pages)

⏱️ **First deployment**: ~10 minutes (Rust/WASM compilation)  
⏱️ **Subsequent deployments**: ~3 minutes (cached)

---

## Part 3B: Manual Deployment (For Testing)

Use this to deploy from any branch without merging to main.

### Step 1: Open Actions Tab

1. Go to the **Actions** tab in your repository

### Step 2: Select Manual Workflow

1. In the left sidebar under **All workflows**, find:
   - **Deploy to GitHub Pages (Manual)**
2. Click on it

### Step 3: Run Workflow

1. Click the **Run workflow** dropdown button (top right)
2. A form appears:
   - **Use workflow from**: Select your branch from dropdown
   - **Deploy branch**: Optionally specify a different branch (or leave empty for current branch)
3. Click the green **Run workflow** button

### Step 4: Monitor Deployment

1. The workflow will appear in the list below
2. Click on it to monitor progress
3. Wait for all steps to complete (green checkmarks)

✅ **Deployment complete!**

---

## Part 4: Access Your Deployed Site

### Your Site URL

Your site will be available at:

```
https://<your-username>.github.io/scramjet/
```

**Example**: If your username is `johndoe`, the URL is:
```
https://johndoe.github.io/scramjet/
```

### First Deployment Notes

- **Wait 2-5 minutes** after the workflow completes for the site to be available
- Your browser might cache the 404 page - try:
  - Hard refresh: `Ctrl+Shift+R` (Windows/Linux) or `Cmd+Shift+R` (Mac)
  - Open in private/incognito window
  - Clear browser cache

---

## Verification Checklist

After deployment, verify everything works:

- [ ] Site loads at `https://<your-username>.github.io/scramjet/`
- [ ] Scramjet demo interface appears
- [ ] TypeDoc documentation is available at `/typedoc/`
- [ ] No console errors in browser developer tools

---

## Troubleshooting Common Issues

### Issue 1: Workflow Doesn't Run

**Symptom**: No workflow appears in Actions tab after push

**Solutions**:
1. Verify you pushed to the `main` branch
2. Check if Actions are enabled (Part 2)
3. Look for the workflow file: `.github/workflows/main.yml` exists
4. Try the manual workflow instead

---

### Issue 2: Deployment Job is Skipped

**Symptom**: Build succeeds but "Pages" job shows "Skipped"

**Cause**: The automatic workflow only runs the Pages job on the `main` branch

**Solutions**:
1. Use the manual deployment workflow for other branches
2. Merge your changes to `main`
3. Or modify the workflow to remove the branch restriction (not recommended for forks)

---

### Issue 3: Permission Errors

**Symptom**: Error like "Resource not accessible by integration"

**Solutions**:
1. Go to **Settings** → **Actions** → **General**
2. Under **Workflow permissions**:
   - Select **Read and write permissions**
   - Check **Allow GitHub Actions to create and approve pull requests**
3. Save changes
4. Re-run the workflow

---

### Issue 4: Site Shows 404

**Symptom**: Workflow succeeds but site shows "404 - File not found"

**Solutions**:
1. **Wait**: First deployment can take 5-10 minutes to propagate
2. **Check Pages Settings**: Ensure source is "GitHub Actions"
3. **Hard Refresh**: Clear cache and reload (Ctrl+Shift+R)
4. **Check Deployment**: Go to **Settings** → **Pages**, look for green checkmark and site URL
5. **Verify Build**: Check Actions logs to ensure `staticbuild` directory was created

---

### Issue 5: Build Fails

**Symptom**: Workflow fails during build step

**Common Causes & Solutions**:

1. **Rust/WASM Tools Error**
   - First-time builds need to install Rust toolchain
   - This is normal and may take longer
   - Re-run the workflow if it times out

2. **Dependencies Error**
   - Run `pnpm install` locally to test
   - Check `package.json` for syntax errors

3. **Out of Memory**
   - Free runners have memory limits
   - Reduce concurrent builds in workflow if needed

---

## Advanced: Custom Configuration

### Changing Bare/WISP Server URLs

The deployment uses default proxy server URLs. To customize:

1. Edit `ci/build-static.sh`
2. Find line 21:
   ```bash
   printf 'let _CONFIG = %s' "$(jq -c -n '{"wispurl": "wss://anura.pro/", "bareurl": "https://aluu.xyz/bare/"}')" > $DST/config.js
   ```
3. Change the URLs to your own servers
4. Commit and push to trigger redeployment

---

## Getting Help

Still having issues?

1. **Check Logs**: Go to Actions tab → Click on failed workflow → Review error messages
2. **Documentation**: Read the full guide at `docs/contributors/GitHubPages.md`
3. **GitHub Issues**: Open an issue describing your problem
4. **Community**: Ask in discussions or Discord

---

## Success! 🎉

Once deployment is successful:

- ✅ Your Scramjet demo is live on GitHub Pages
- ✅ Accessible worldwide at your GitHub Pages URL
- ✅ Automatically updates when you push to main
- ✅ Includes full API documentation

**Next Steps**:
- Share your deployed site
- Customize the configuration for your needs
- Set up a custom domain (optional)

---

*This guide is maintained in the scramjet repository. Last updated: 2026-02-12*
