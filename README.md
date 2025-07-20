# Omarchy Theme Example

A template repository for creating and publishing Omarchy theme packages to the AUR (Arch User Repository).

## 🚀 Quick Start - Using This Template

### 1. Create Your Repository

1. Click "Use this template" button on GitHub
2. Name your repo: `omarchy-theme-yourname` (replace `yourname` with your theme name)
3. Clone your new repository:
   ```bash
   git clone https://github.com/yourusername/omarchy-theme-yourthemename
   cd omarchy-theme-yourthemename
   ```

### 2. Customize Your Theme

1. **Edit theme files** in the `theme_files/` directory:
   - `hyprland.conf` - Window manager configuration
   - `waybar.css` - Status bar styling
   - `alacritty.toml` - Terminal colors
   - `backgrounds/` - Add your wallpapers here
   - And more...

2. **Update package metadata** in `PKGBUILD`:
   ```bash
   # Edit these lines:
   # Maintainer: Your Name <your@email.address>
   pkgname=omarchy-theme-yourthemename
   pkgdesc="Your theme description for Omarchy"
   url="https://github.com/yourusername/omarchy-theme-yourthemename"
   ```

### 3. Set Up GitHub Secrets

Go to your GitHub repository → Settings → Secrets and variables → Actions

Add these three secrets:

1. **AUR_USERNAME**: Your AUR username
2. **AUR_EMAIL**: Your email (e.g., `user@example.com`)
3. **AUR_SSH_PRIVATE_KEY**: 
   ```bash
   # Generate a new SSH key for AUR -- Don't reuse one you already have.
   ssh-keygen -t ed25519 -f ~/.ssh/aur_github_actions -C "aur-github-actions"
   
   # Copy this private key as your secret:
   cat ~/.ssh/aur_github_actions
   
   # Add the public key to your AUR account:
   cat ~/.ssh/aur_github_actions.pub
   ```
   
   Add the public key at: https://aur.archlinux.org/account

### 4. Publish Your First Release

```bash
# Commit all your changes
git add .
git commit -m "Initial theme release"

# Create and push a version tag
git tag -a v1.0.0 -m "Initial release"
git push origin main --tags
```

That's it! GitHub Actions will automatically update the AUR package whenever you create a new tag.

## 📦 For End Users

### Installing Themes

```bash
# Install using yay
yay -S omarchy-theme-example
```

### Using Themes

After installation, themes are available in `/usr/share/omarchy/themes/` and symlinked to the appropriate directories in `~/.config/omarchy/`. Select your theme through `Super + Shift + Space`

## 🔄 Releasing Updates

When you want to release a new version:

1. Make your changes to the theme files
2. Commit and push to GitHub
3. Create a new version tag:
   ```bash
   git tag -a v1.0.1 -m "Update colors"
   git push origin --tags
   ```

The GitHub Action automatically:
- Updates the version in PKGBUILD
- Publishes to AUR
- Users can update with `yay -Syu`

## 📁 Repository Structure

```
omarchy-theme-yourname/
├── theme_files/           # Your theme files
│   ├── hyprland.conf     # Hyprland window manager config
│   ├── hyprlock.conf     # Lock screen configuration
│   ├── waybar.css        # Status bar styling
│   ├── walker.css        # Application launcher theme
│   ├── alacritty.toml    # Terminal colors
│   ├── mako.ini          # Notification daemon styling
│   ├── btop.theme        # System monitor theme
│   ├── neovim.lua        # Neovim colorscheme
│   └── backgrounds/      # Wallpaper directory
│       └── 1-example.png # Example wallpaper (number prefix for ordering)
├── PKGBUILD              # AUR package definition
├── *.install             # Post-install messages
├── .github/
│   └── workflows/
│       └── aur-publish.yml  # Automation workflow
└── README.md             # This file
```

## 🛠️ Troubleshooting

### Package not found after publishing
- Wait 5-10 minutes for AUR database to sync
- Try: `yay -Sy` to refresh package database
- Use full name: `yay -S aur/omarchy-theme-yourname`

### GitHub Action fails
- Check your secrets are set correctly
- Ensure your SSH key is added to your AUR account
- Check Actions tab for detailed error messages

### Version conflicts
- Always tag with 'v' prefix: `v1.0.0`, not `1.0.0`
- Reset `pkgrel=1` when updating `pkgver`

## 📄 License

This template is provided under the MIT license. Your theme can use any license you prefer.
