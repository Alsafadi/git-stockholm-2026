# Installing and Configuring Git

_Setting up your Git environment_

---

### Why Install Git?

Git is a **command-line tool** that needs to be installed on your system

**Different from:**

- GitHub (web platform)
- Git GUIs (visual interfaces)
- IDE integrations (VS Code, IntelliJ)

**Git is the engine** that powers all these tools

---

### Installation Options (Windows)

- **Git for Windows** (recommended)
- **GitHub Desktop** (includes Git)
- **Windows Subsystem for Linux (WSL)**

---

### Installation Options (macOS)

- **Homebrew**: `brew install git`
- **Xcode Command Line Tools**
- **Git installer from git-scm.com**

---

### Installation Options (Linux)

- **Ubuntu/Debian**: `sudo apt install git`
- **CentOS/RHEL**: `sudo yum install git`
- **Arch**: `sudo pacman -S git`

---

### Git for Windows Features

#### What you get:

- **Git Bash** - Unix-like terminal
- **Git GUI** - Graphical interface
- **Shell integration** - Right-click context menus
- **Credential Manager** - Windows authentication

#### Installation tips:

- Choose "Git Bash Here" and "Git GUI Here"
- Select "Use Git from Windows Command Prompt"
- Choose "Checkout Windows-style, commit Unix-style line endings"

---

### Verifying Installation

Open terminal/command prompt:

```bash
git --version
```

Expected output:

```
git version 2.47.1.windows.2
```

**If this works, Git is installed! 🎉**

---

### Essential Configuration

After installation, configure your identity:

```bash
# Set your name (used in commits)
git config --global user.name "Your Full Name"

# Set your email (used in commits)
git config --global user.email "your.email@example.com"
```

**These appear in every commit you make**

---

### Configuration Levels

Git has three configuration levels:

- System (--system)
- Global (--global)
- Local (--local)

---

#### System (--system)

- Applies to all users on the computer
- Location: `/etc/gitconfig`

#### Global (--global)

- Applies to your user account
- Location: `~/.gitconfig`

#### Local (--local)

- Applies to specific repository
- Location: `.git/config`

---

**Priority:** Local > Global > System

---

### Essential Global Settings

```bash
# Your identity
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# Default branch name
git config --global init.defaultBranch main

# Line ending handling
git config --global core.autocrlf true   # Windows
git config --global core.autocrlf input  # macOS/Linux

# Default editor
git config --global core.editor "code --wait"  # VS Code
```

---

### Useful Optional Settings

```bash
# Colorful output
git config --global color.ui auto

# Better diff algorithm
git config --global diff.algorithm histogram

# Remember credentials (Windows)
git config --global credential.helper manager-core

# Remember credentials (macOS)
git config --global credential.helper osxkeychain

# Aliases for common commands
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
```

---

### Checking Your Configuration

View all settings:

```bash
git config --list
```

View specific setting:

```bash
git config user.name
git config user.email
```

View configuration file location:

```bash
git config --list --show-origin
```

---

### SSH Key Setup (Optional but Recommended)

For GitHub/GitLab without password prompts:

#### 1. Generate SSH key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

#### 2. Add to SSH agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

#### 3. Add public key to GitHub

Copy `~/.ssh/id_ed25519.pub` content to GitHub → Settings → SSH Keys

---

### SSH Key Setup (Optional but Recommended)

For GitHub/GitLab without password prompts:

#### 1. Generate SSH key

```powershell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

**When prompted:**

- Press Enter for default location (`C:\Users\YourName\.ssh\id_ed25519`)
- Enter passphrase (optional but recommended)

#### 2. Start SSH agent and add key

```powershell
# Start the ssh-agent service
Start-Service ssh-agent

# Add your SSH key to the agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

#### 3. Copy public key to clipboard

```powershell
# Copy public key content to clipboard
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard
```

#### 4. Add public key to GitHub

1. Go to GitHub → Settings → SSH and GPG Keys
2. Click "New SSH Key"
3. Paste the key (Ctrl+V) and give it a descriptive title
4. Click "Add SSH Key"

#### 5. Test SSH connection

```powershell
ssh -T git@github.com
```

**Expected output:**

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

#### Command Line

- **Git Bash** (Windows)
- **Terminal** (macOS/Linux)
- **PowerShell/CMD** (Windows)

---

### Git GUI Options

#### Graphical Interfaces

- **GitHub Desktop** - Simple, GitHub-focused
- **GitKraken** - Feature-rich, cross-platform
- **SourceTree** - Atlassian's free client
- **Git Extensions** - Windows-focused
- **VS Code** - Built-in Git integration
- **Git Tower** - Commercial Git client for windows and MacOS

---

### IDE Integration

Most modern IDEs include Git support:

#### VS Code

- Built-in Git integration
- GitLens extension for advanced features

#### IntelliJ/PyCharm/WebStorm

- Comprehensive Git tools
- Visual merge conflict resolution

#### Sublime Text, Atom, Vim

- Various Git plugins available

**Start with command line, then add GUI tools**

---

### Troubleshooting Common Issues

#### "git: command not found"

- Git not installed or not in PATH
- Restart terminal after installation

#### "Please tell me who you are"

- Missing user.name or user.email
- Run the configuration commands

#### HTTPS vs SSH authentication

- HTTPS: Username/password or token
- SSH: Key-based authentication (recommended)

---

### Configuration Best Practices

#### ✅ Do:

- Set up user.name and user.email immediately
- Use consistent email across platforms
- Set up SSH keys for frequent use
- Choose one primary tool and learn it well

#### ❌ Avoid:

- Using different emails for work/personal
- Skipping initial configuration
- Installing multiple Git versions

---

### Demo Time! 🎬

Let's configure Git together:

1. **Check Git installation**
2. **Set up user identity**
3. **Configure essential settings**
4. **Verify configuration**
5. **Test with first repository**

---

## Next Steps

✅ **Git is installed and configured**

**Coming up:**

- Configuring .gitignore and .gitattributes
- Core Git concepts
- Your first repository

**Questions about installation or configuration?**
