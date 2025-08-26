# Setup ZSH Development Environment

A modern Docker-based development environment with Oracle Linux 9 and ZSH best practice plugins, managed through Taskfile for simplicity.

## Features

- **Oracle Linux 9** base image with minimal footprint
- **ZSH with Zinit** plugin manager (fast turbo-mode loading)
- **Starship prompt** (cross-shell, fast, and customizable)
- **Mise runtime version manager** (modern alternative to asdf/nvm/pyenv)
- **Modern terminal tools:**
  - eza (modern ls replacement)
  - bat (syntax highlighting cat)
  - fd (user-friendly find)
  - ripgrep (fast grep)
  - fzf (fuzzy finder)
  - zoxide (smart cd)
- **ZSH plugins with Zinit:**
  - zsh-autosuggestions (intelligent command suggestions)
  - fast-syntax-highlighting (syntax highlighting)
  - zsh-completions (enhanced completions)
  - zsh-history-substring-search (better history search)
  - zsh-autopair (automatic bracket pairing)
  - zsh-you-should-use (alias reminder)
- **Essential tools:** Git, Vim/Neovim, Tmux, JQ, Tree
- **Taskfile-based management** (no complex scripts needed)
- **Persistent history and settings**

## Quick Start

### 1. Setup Environment

```bash
# Clone or download this repository
git clone <repository-url>
cd setup-zsh
```

### 2. Start the Environment

```bash
# Start the development environment
task up

# Build and start (first time)
task build && task up
```

### 3. Connect to Shell

```bash
# Connect to ZSH shell
task shell
```

## Available Commands

### Taskfile Commands

```bash
task up        # Start the development environment
task down      # Stop the development environment
task build     # Build the Docker image
task rebuild   # Rebuild and restart
task shell     # Connect to ZSH shell
task bash      # Connect to bash (fallback)
task logs      # View container logs
task restart   # Restart the environment
task stop      # Stop the environment
task status    # Show container status
task clean     # Clean up containers and images

# Mise runtime management
task mise-list        # List installed runtimes
task mise-setup       # Setup common runtimes (Node.js, Python, Go)
task mise-install -- node@lts    # Install specific runtime
task mise-use -- python@3.12     # Set global runtime version
task mise -- --help              # Run any mise command
```

## Directory Structure

```
.
├── docker/
│   ├── Dockerfile              # Oracle Linux 9 + ZSH + terminal tools
│   ├── mise/
│   │   └── .mise.toml         # Mise configuration
│   └── zsh/
│       ├── .zshrc             # ZSH configuration with Zinit
│       ├── .zinit_config      # Zinit plugin configuration
│       ├── .zsh_aliases       # Custom aliases and functions
│       └── starship.toml      # Starship prompt configuration
├── docker-compose.yml         # Container orchestration
├── Taskfile.yml              # Task runner configuration
├── .tool-versions            # Mise tool versions (example)
├── .env.example              # Environment variables template
├── .gitignore                # Git ignore rules
├── .dockerignore             # Docker ignore rules
└── README.md                 # This file
```

## Included Tools and Aliases

### Modern CLI Tools
- `eza` - Modern replacement for ls with git integration
- `bat` - Syntax highlighting for cat
- `fd` - User-friendly find alternative
- `ripgrep (rg)` - Fast recursive search
- `fzf` - Fuzzy finder for interactive searching
- `zoxide (z)` - Smart directory jumping
- `jq` - JSON processor
- `tree` - Directory tree viewer

### Git Aliases
- `gs` → `git status`
- `ga` → `git add`
- `gc` → `git commit`
- `gp` → `git push`
- `gl` → `git pull`
- `gb` → `git branch`
- `gco` → `git checkout`
- `gd` → `git diff`
- `glog` → `git log --oneline --graph --decorate`

### Docker Aliases
- `d` → `docker`
- `dc` → `docker compose`
- `dcu` → `docker compose up`
- `dcd` → `docker compose down`
- `dcb` → `docker compose build`
- `dps` → `docker ps`
- `di` → `docker images`

### Useful Functions
- `mkcd <dir>` - Create directory and cd into it
- `extract <file>` - Extract various archive formats
- `weather <city>` - Get weather information
- `localip` - Show local IP address
- `externalip` - Show external IP address
- `sysinfo` - Show detailed system information

## Customization

### Adding Custom Aliases

Edit `docker/zsh/.zsh_aliases` and rebuild:

```bash
task rebuild
```

### Adding More Plugins

Edit `docker/zsh/.zinit_config` to add more plugins, then rebuild:

```bash
task rebuild
```

### Starship Prompt Customization

Edit `docker/zsh/starship.toml` to customize the prompt, then rebuild:

```bash
task rebuild
```

## Mise Runtime Management

Mise is a modern runtime version manager that replaces tools like nvm, pyenv, rbenv, etc.

### Quick Setup

```bash
# Setup common development runtimes
task mise-setup

# Or install specific runtimes
task mise-install -- node@lts
task mise-install -- python@3.12
task mise-install -- go@latest
```

### Using Mise

```bash
# List available versions
task shell
mise ls-remote node

# Install specific version
mise install node@20.10.0

# Set global version
mise use -g node@20.10.0

# Set local version for project
mise use node@18.18.0

# List installed versions
mise ls
```

### Project-specific Versions

Create a `.tool-versions` file in your project:

```
nodejs 20.10.0
python 3.12.1
golang 1.21.5
```

Mise will automatically switch to these versions when you enter the directory.

### Mise Configuration

The mise configuration is stored in `docker/mise/.mise.toml`:

- Global settings and preferences
- Tool aliases (e.g., `lts` → `20` for Node.js)
- Environment variables
- Default tools to install

### Zinit Plugin Management

Zinit automatically manages plugin installation, updates, and loading. All plugins are configured in `.zinit_config` with turbo-mode loading for fast startup times.

### Local Configuration

Create `~/.zshrc.local` inside the container for personal settings that won't be overwritten.

## Volume Mounts

- `./:/workspace` - Current directory mounted as workspace
- `zsh_history:/home/devuser/.zsh_history_data` - Persistent ZSH history
- `zsh_cache:/home/devuser/.cache` - Persistent cache
- `~/.ssh:/home/devuser/.ssh:ro` - SSH keys (read-only)
- `~/.gitconfig:/home/devuser/.gitconfig:ro` - Git configuration (read-only)

## Troubleshooting

### Container Won't Start

```bash
# Check Docker is running
docker info

# Check logs
task logs

# Rebuild image
task rebuild
```

### Permission Issues

### Plugin Issues

```bash
# Connect to shell and update plugins manually
task shell
zinit update
```

## Contributing

1. Fork the repository
2. Create your feature branch
3. Make your changes
4. Test with `task rebuild`
5. Submit a pull request

## License

This project is open source and available under the [MIT License](LICENSE).
