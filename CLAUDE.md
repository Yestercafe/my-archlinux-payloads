# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a personal Arch Linux desktop configuration repository (dotfiles) containing a cohesive desktop environment built around:
- **Niri**: A scrollable-tiling Wayland compositor (configured in KDL format)
- **Noctalia**: A custom desktop shell/extension system providing a bar, dock, notifications, and system controls
- **Ghostty**: Modern terminal emulator
- **Emacs**: Text editor with custom theming

The entire setup uses a consistent "Noctalia" color scheme with warm coral/pink tones on a dark background.

## Directory Structure

```
.
├── .config/
│   ├── niri/           # Wayland compositor configuration (KDL format)
│   │   ├── config.kdl  # Main niri config with keybindings, outputs, layout
│   │   └── noctalia.kdl # Noctalia theme overrides (colors, focus rings)
│   ├── ghostty/        # Terminal emulator configuration
│   ├── noctalia/       # Desktop shell configuration (JSON)
│   │   ├── settings.json    # Main Noctalia configuration
│   │   ├── colors.json      # Color scheme definitions
│   │   └── plugins.json     # Plugin sources
│   └── (other apps)
└── .emacs.d/           # Emacs configuration
    ├── init.el         # Main Emacs init file
    └── themes/         # Custom themes (includes noctalia-theme.el)
```

## Key Configuration Concepts

### Niri (Wayland Compositor)

Niri uses **KDL** (a document language) for configuration. The main configuration is in `.config/niri/config.kdl`:

- **Output configuration**: Multi-monitor setup with different scaling factors (see lines 84-125)
- **Layout**: Tiling window management with gaps, focus rings, and borders (lines 130-281)
- **Keybindings**: Extensive keybinding system using `Mod` (Super/Alt depending on context) (lines 370-644)
- **Window rules**: Per-window behavior overrides (lines 362-368)
- **Startup**: Launches `qs -c noctalia-shell` on startup (line 293)

The Niri config uses a modular KDL include system. Color themes are defined in `noctalia.kdl` and included via `include "./noctalia.kdl"` at the end of `config.kdl`. This separates structural configuration from theming.

### Noctalia (Desktop Shell)

Noctalia is configured via JSON files in `.config/noctalia/`:

- **settings.json**: Comprehensive configuration covering:
  - Bar widgets (workspaces, system monitor, clock, audio, media)
  - Dock (auto-hiding application launcher)
  - Control center (quick settings panel)
  - Notifications with history
  - App launcher (fuzzel-based with clipboard history)
  - Screen recording
  - Wallpaper management
  - Color scheme generation (using Matugen)

- **colors.json**: Defines the Material Design 3 color palette used across the entire desktop

### Theme System

The "Noctalia" color scheme is applied consistently across:
- Niri (focus rings, borders, shadows)
- Ghostty terminal (via `theme = noctalia`)
- Emacs (via `noctalia-theme.el`)
- Noctalia shell (via colors.json)

Key colors:
- Primary: `#ffb3af` (soft coral/pink)
- Background: `#1b1206` (dark warm brown)
- On Surface: `#f3dfcb` (light warm cream)
- Tertiary: `#f7bc70` (warm amber)

Noctalia's `templates` setting (in settings.json) controls which apps receive theming. When enabled, it generates configs for: niri, ghostty, gtk, qt, fuzzel, etc.

### Emacs Configuration

The Emacs setup (`.emacs.d/init.el`) is minimal:
- Disables toolbar, menubar, scrollbar
- Uses `Sarasa Mono SC` font
- Loads custom `noctalia-theme`
- Relative line numbers enabled
- Backup files disabled

The theme file (`.emacs.d/themes/noctalia-theme.el`) provides extensive syntax highlighting for org-mode, Magit, Company, and various programming modes.

## Common Operations

### Deploying Configuration

The repository includes a `deploy` script for managing dotfile symlinks (similar to GNU Stow):

```bash
# Deploy all dotfiles to home directory
./deploy install

# Dry run to see what would be done
./deploy install -n

# Show deployment status
./deploy status

# Remove symlinks and restore backups
./deploy undeploy

# Verbose mode
./deploy install -v
```

The deploy script:
- Creates symlinks from `.config/`, `.emacs.d/` to `~/.config/`, `~/.emacs.d`
- Downloads wallpapers from `wallpapers.txt` to `~/Pictures/Wallpapers/`
- Backs up existing files before replacing them
- Can restore backups when undeploying

### Reloading Configurations

After making changes:

```bash
# Niri: Reload config (if running niri as session)
niri msg action reload-config

# Ghostty: Reload config
# Use Ctrl+Shift+, (default) or restart Ghostty

# Noctalia: Usually auto-reloads, or restart via session
# Emacs: Reload theme with M-x load-theme
```

### Testing Theme Changes

When modifying color schemes:

1. Update `.config/noctalia/colors.json` for the shell
2. Update `.config/niri/noctalia.kdl` for Niri
3. Update `.emacs.d/themes/noctalia-theme.el` for Emacs
4. If using Matugen, theme templates may regenerate automatically

## Security Considerations

The `.gitignore` excludes sensitive configuration files:
- `*.conf` files (may contain passwords/tokens)
- `password.conf`
- Auto-generated files (`.elc`, backup files)

Never commit files containing API keys, passwords, or personal tokens.

## Keybindings

Niri uses `Mod` as the primary modifier (Super on TTY, Alt in windowed mode). Keybindings are defined in `.config/niri/config.kdl`:

- **Mod+T**: Open Ghostty terminal
- **Mod+D**: Open fuzzel app launcher
- **Mod+Q**: Close window
- **Mod+O**: Toggle overview
- **Mod+H/J/K/L** or Arrow keys: Focus windows/columns
- **Mod+Ctrl+H/J/K/L**: Move windows/columns
- **Mod+1-9**: Focus workspace by index
- **Mod+Ctrl+1-9**: Move column to workspace
- **Mod+R**: Cycle preset column widths
- **Mod+F**: Maximize column
- **Mod+Shift+F**: Fullscreen window
- **Mod+V**: Toggle floating
- **Mod+W**: Toggle tabbed column display
- **Print/Ctrl+Print/Alt+Print**: Screenshot (screen/window)
- **Media keys**: Volume, brightness, playback control (works when locked)

## Technology Stack

- **Display Server**: Wayland (via Niri)
- **Input Method**: fcitx (configured in niri environment)
- **Audio**: PipeWire & WirePlumber (wpctl for volume control)
- **Media**: playerctl (MPRIS-compatible media control)
- **Screenshots**: Built-in Niri screenshot functionality
- **Screen Recording**: Noctalia screen recorder (portal-based, saves to `~/Videos`)
- **Fonts**: Sarasa Mono SC (terminal), Noto Sans (UI)
- **Shell**: qs (Quick Shell, running Noctalia shell)

## Development Notes

- This is a **personal configuration repository**, not a software project
- Changes should be tested before committing
- The Noctalia shell appears to be a custom QS (Quick Shell) configuration
- All configs follow XDG Base Directory specification (`~/.config/`)
- Niri KDL format uses `//` for comments and `/-` to comment out nodes
