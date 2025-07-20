# AUR Package Setup Guide

This guide explains how to set up automated AUR publishing for your package.

## Repository Structure

This repository uses a **single-repo approach** where:
- The main code/theme files live in the repository
- PKGBUILD is in the root directory
- GitHub Actions automatically publishes to AUR when you create a release

## Initial Setup

### 1. Set up GitHub Secrets

Go to your GitHub repository Settings → Secrets and variables → Actions, and add:

- `AUR_USERNAME`: Your AUR username
- `AUR_EMAIL`: Your AUR email (e.g., `username@domain.com`)
- `AUR_SSH_PRIVATE_KEY`: Your SSH private key for AUR (see below)

### 2. Generate an SSH Key for AUR

```bash
# Generate a new SSH key specifically for AUR
ssh-keygen -t ed25519 -f ~/.ssh/aur_github_actions -C "github-actions-aur"

# Copy the public key
cat ~/.ssh/aur_github_actions.pub
```

Add the public key to your AUR account at: https://aur.archlinux.org/account

Copy the private key to use as the `AUR_SSH_PRIVATE_KEY` secret:
```bash
cat ~/.ssh/aur_github_actions
```

### 3. Initial Manual Push to AUR

Before automation works, you need to create the package on AUR:

```bash
# Add AUR remote
git remote add aur ssh://aur@aur.archlinux.org/omarchy-theme-example.git

# Generate .SRCINFO
makepkg --printsrcinfo > .SRCINFO

# Create a temporary branch for AUR
git checkout -b aur-initial
git add PKGBUILD .SRCINFO
git commit -m "Initial AUR package"

# Push to AUR (this creates the package)
git push aur aur-initial:master

# Go back to main branch
git checkout main
git branch -d aur-initial
```

## Releasing New Versions

After initial setup, releasing is simple:

```bash
# Update your code/theme files
git add .
git commit -m "Update theme"

# Tag a new version
git tag -a v1.0.1 -m "Release version 1.0.1"

# Push to GitHub (this triggers the workflow)
git push origin main --tags
```

The GitHub Action will automatically:
1. Update the `pkgver` in PKGBUILD to match your tag
2. Generate a new .SRCINFO
3. Push the updates to AUR

## How It Works

1. **Single Repository**: Your GitHub repo contains everything
2. **GitHub Auto-archives**: When you tag a release, GitHub automatically creates source tarballs
3. **PKGBUILD downloads from GitHub**: The PKGBUILD references the GitHub release tarball
4. **Automation**: GitHub Actions handles all AUR updates when you push tags

## Best Practices

1. **Versioning**: Always use semantic versioning with 'v' prefix (e.g., v1.0.0, v1.0.1)
2. **Testing**: Test your PKGBUILD locally before releasing:
   ```bash
   makepkg -si
   ```
3. **Commit Messages**: Use clear commit messages for releases
4. **Don't Commit .SRCINFO**: Let the automation handle it to avoid conflicts

## Troubleshooting

- **Authentication Failed**: Check your SSH key is correctly added to AUR and GitHub secrets
- **Package Already Exists**: You need to adopt or request deletion of existing packages
- **Workflow Fails**: Check the Actions tab in GitHub for detailed error logs