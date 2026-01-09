# arch-dotfiles

> **Powered by [Claude Code](https://claude.ai/code)** — A personal Arch Linux desktop configuration crafted with AI assistance.

A cohesive Wayland-based desktop environment built around **Niri** (scrollable-tiling compositor) and **Noctalia** (custom desktop shell), featuring a warm coral/pink "Noctalia" color scheme across all applications.

## Preview

The desktop features a clean, modern aesthetic with:
- Floating bar with workspace, system monitor, clock, and media widgets
- Auto-hiding dock for application launching
- Rounded window corners with coral focus rings
- Consistent warm coral/pink theming across all applications

## Components

| Component | Description |
|-----------|-------------|
| **Niri** | Scrollable-tiling Wayland compositor with smart column layout |
| **Noctalia** | Custom desktop shell providing bar, dock, notifications, and control center |
| **Ghostty** | Modern terminal emulator with GPU acceleration |
| **Emacs** | Text editor with custom Noctalia theme |

## Theme

The entire setup uses a consistent "Noctalia" color scheme with warm coral/pink tones on a dark background:

- **Primary**: `#ffb3af` (soft coral/pink)
- **Background**: `#1b1206` (dark warm brown)
- **On Surface**: `#f3dfcb` (light warm cream)
- **Tertiary**: `#f7bc70` (warm amber)

Color theming is applied via Noctalia's template system to: Niri, Ghostty, GTK, Qt, Fuzzel, and more.

## Installation

```bash
# Clone the repository
git clone <repo-url> ~/arch-dotfiles
cd ~/arch-dotfiles

# Deploy all dotfiles (creates symlinks, downloads wallpapers)
./deploy install

# Dry run to see what would be done
./deploy install -n

# Check deployment status
./deploy status
```

The deploy script will:
- Create symlinks from `.config/` to `~/.config/`
- Create symlinks from `.emacs.d/` to `~/.emacs.d/`
- Download wallpapers from `wallpapers.txt` to `~/Pictures/Wallpapers/`
- Backup any existing files before replacing them

## Keybindings

### Window Management
- `Mod+T` — Open Ghostty terminal
- `Mod+D` — Open fuzzel app launcher
- `Mod+Q` — Close window
- `Mod+H/J/K/L` or Arrows — Focus windows/columns
- `Mod+Ctrl+H/J/K/L` — Move windows/columns
- `Mod+V` — Toggle floating
- `Mod+F` — Maximize column
- `Mod+Shift+F` — Fullscreen window
- `Mod+W` — Toggle tabbed column display
- `Mod+R` — Cycle preset column widths

### Workspace Management
- `Mod+1-9` — Focus workspace by index
- `Mod+Ctrl+1-9` — Move column to workspace
- `Mod+U/I` or Page Down/Up — Focus workspace down/up
- `Mod+O` — Toggle overview

### Media & System
- `Print` — Screenshot
- `Ctrl+Print` — Screenshot screen
- `Alt+Print` — Screenshot window
- `XF86AudioRaiseVolume/LowerVolume/Mute` — Volume control
- `XF86MonBrightnessUp/Down` — Brightness control
- `XF86AudioPlay/Stop/Prev/Next` — Media playback

*Note: `Mod` is Super on TTY, Alt in windowed mode.*

## Requirements

- **Arch Linux** (or compatible distribution)
- **Niri** — Wayland compositor
- **Noctalia** — Desktop shell (QS/Quick Shell based)
- **Ghostty** — Terminal emulator
- **PipeWire & WirePlumber** — Audio
- **playerctl** — Media control
- **fcitx** — Input method
- **qs** — Quick Shell (for Noctalia)

## Configuration

After deployment, reload configurations:

```bash
# Niri
niri msg action reload-config

# Ghostty — Ctrl+Shift+, or restart

# Noctalia — Auto-reloads or restart session

# Emacs — M-x load-theme
```

See [CLAUDE.md](./CLAUDE.md) for detailed configuration guidance.

## Structure

```
.
├── .config/
│   ├── niri/           # Niri compositor config (KDL)
│   │   ├── config.kdl  # Main configuration
│   │   └── noctalia.kdl # Color theme
│   ├── ghostty/        # Terminal config
│   └── noctalia/       # Desktop shell config (JSON)
│       ├── settings.json    # Main settings
│       ├── colors.json      # Color palette
│       └── plugins.json     # Plugin sources
├── .emacs.d/           # Emacs config
│   ├── init.el         # Init file
│   └── themes/         # Custom themes
├── deploy              # Deployment script
└── wallpapers.txt      # Wallpaper download list
```

## License

This is a personal configuration repository. Feel free to adapt and use for your own setup.

---

Developed with assistance from **Claude Code** — Anthropic's AI-powered CLI for software engineering.
