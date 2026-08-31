# Configuring .gitignore and .gitattributes

_Controlling what Git tracks and how_

---

## Why Do We Need .gitignore?

**Not everything should be in version control:**

- ❌ **Build artifacts** (compiled code, dist folders)
- ❌ **Dependencies** (node_modules, vendor)
- ❌ **Temporary files** (logs, cache, swap files)
- ❌ **Secrets** (API keys, passwords, certificates)
- ❌ **OS files** (.DS_Store, Thumbs.db)
- ❌ **IDE files** (.vscode, .idea)

**Git tracks everything by default** - we need to tell it what to ignore

---

## What is .gitignore?

A special file that tells Git which files and directories to **never track**

### Key points:

- **File name**: `.gitignore` (note the leading dot)
- **Location**: Usually in repository root
- **One per repository** (can have nested ones)
- **Tracked by Git** (yes, we track the ignore file!)
- **Plain text** with simple patterns

---

## .gitignore Syntax

### Basic patterns:

```bash
# Comments start with #
temp.txt          # Ignore specific file
*.log             # Ignore all .log files
build/            # Ignore entire directory
docs/*.pdf        # Ignore PDFs in docs folder only
!important.log    # Exception: don't ignore this file
```

### Advanced patterns:

```bash
**/*.cache        # Ignore .cache files in any subdirectory
temp*             # Ignore files starting with "temp"
*.{jpg,png,gif}   # Ignore multiple extensions
/root-only.txt    # Only ignore in repository root
```

---

## Common .gitignore Examples

### Node.js project:

```bash
node_modules/
npm-debug.log
.env
dist/
build/
.nyc_output/
coverage/
```

### Python project:

```bash
__pycache__/
*.pyc
*.pyo
.pytest_cache/
venv/
.env
*.egg-info/
.coverage
```

---

## More .gitignore Examples

### Java project:

```bash
*.class
*.jar
*.war
target/
.gradle/
build/
.project
.classpath
```

### General development:

```bash
# OS files
.DS_Store
Thumbs.db
desktop.ini

# IDE files
.vscode/
.idea/
*.swp
*.swo
```

---

## Creating .gitignore

### Method 1: Manual creation

```bash
# Create file
touch .gitignore

# Edit file
nano .gitignore
# or
code .gitignore
```

### Method 2: Using templates

Visit [gitignore.io](https://gitignore.io)

- Select your language/framework
- Download ready-made .gitignore

### Method 3: Platform templates

When creating a repository, GitHub, GitLab, and Bitbucket **all** offer a
language dropdown that pre-fills a `.gitignore` for you

---

## .gitignore Best Practices

### ✅ Do:

- **Add .gitignore early** - before first commit
- **Use wildcards** for file types
- **Include comments** for clarity
- **Keep it organized** by category
- **Test your patterns** before committing

### ❌ Avoid:

- Ignoring too broadly (might miss important files)
- Ignoring files already tracked (remove first!)
- Platform-specific ignores in shared repos

---

## Already Tracked Files

**Problem:** File is already in Git but now you want to ignore it

**Solution:** Remove from tracking but keep locally

```bash
# Remove from Git tracking but keep file
git rm --cached filename.txt

# Remove entire directory from tracking
git rm -r --cached directory/

# Then add to .gitignore
echo "filename.txt" >> .gitignore

# Commit changes
git add .gitignore
git commit -m "Stop tracking filename.txt"
```

---

## What are .gitattributes?

Controls **how Git handles files** beyond just tracking

### Key functions:

- **Line ending normalization** (CRLF vs LF)
- **Binary file detection**
- **Text encoding**
- **Merge strategies**
- **Export behavior**
- **Custom diff/merge tools**

---

## .gitattributes Syntax

```bash
# Pattern followed by attributes
*.txt           text
*.jpg           binary
*.sh            text eol=lf
*.bat           text eol=crlf
*.json          text
*.md            text
Dockerfile      text

# All text files get normalized line endings
* text=auto
```

---

## Line Endings: The Cross-Platform Problem

### The issue:

- **Windows**: CRLF (`\r\n`)
- **Unix/Mac**: LF (`\n`)
- **Git**: Can convert automatically

### Common .gitattributes for line endings:

```bash
# Automatically detect text files and normalize
* text=auto

# Force LF for shell scripts
*.sh text eol=lf
*.bash text eol=lf

# Force CRLF for Windows batch files
*.bat text eol=crlf
*.cmd text eol=crlf

# Binary files - no conversion
*.png binary
*.jpg binary
*.ico binary
```

---

## Advanced .gitattributes Examples

### For web development:

```bash
* text=auto

*.html text
*.css text
*.js text
*.json text
*.md text
*.yml text
*.yaml text

*.png binary
*.jpg binary
*.gif binary
*.ico binary
*.woff binary
*.woff2 binary
```

### For documentation projects:

```bash
* text=auto

*.md text
*.txt text
*.pdf binary
*.png binary
*.jpg binary
```

---

## Global vs Local Configuration

### Global .gitignore (for your personal files):

```bash
# Set global ignore file
git config --global core.excludesfile ~/.gitignore_global

# Edit it
nano ~/.gitignore_global
```

Common global ignores:

```bash
# OS files
.DS_Store
Thumbs.db

# Editor files
.vscode/
*.swp
*~
```

**Local .gitignore** goes in each repository for project-specific ignores

---

## Checking What's Ignored

### See what would be ignored:

```bash
git status --ignored
```

### Check if specific file is ignored:

```bash
git check-ignore -v filename.txt
```

### Force add ignored file:

```bash
git add -f filename.txt
```

### List all tracked files:

```bash
git ls-tree -r HEAD --name-only
```

---

## Demo Time! 🎬

Let's set up .gitignore and .gitattributes:

1. **Create a test repository**
2. **Add various file types**
3. **Create .gitignore**
4. **Test ignore patterns**
5. **Add .gitattributes**
6. **See the effects**

---

## When Things Go Wrong

### File still showing up despite .gitignore?

1. **Check if already tracked**:

```bash
git ls-files | grep filename
```

2. **Remove from tracking**:

```bash
git rm --cached filename
```

3. **Check pattern syntax**:

```bash
git check-ignore -v filename
```

### Line ending issues?

1. **Refresh the repository**:

```bash
git add --renormalize .
```

2. **Check attributes**:

```bash
git check-attr text filename
```

---

## Template Resources

### Great starting points:

- **[gitignore.io](https://gitignore.io)** - Custom ignore files, works regardless of host
- **[GitHub templates](https://github.com/github/gitignore)** - Community maintained
- **[GitLab templates](https://gitlab.com/gitlab-org/gitlab/-/tree/master/lib/gitlab/gitignore_templates)** - More options
- **Bitbucket** - no public templates repo to browse, but the "Create repository" wizard offers the same language dropdown as GitHub/GitLab (backed by gitignore.io under the hood)

### Popular combinations:

- Node.js + VS Code
- Python + PyCharm
- Java + IntelliJ
- C# + Visual Studio

---

## Quick Reference

### Common ignore patterns:

```bash
logs/             # Directory
*.log             # Extension
temp*             # Starts with
*temp             # Ends with
temp?.txt         # Single character wildcard
**/*.tmp          # Recursive
!keep-this.log    # Exception
```

### Common attributes:

```bash
* text=auto       # Auto-detect text files
*.sh eol=lf       # Force Unix line endings
*.bat eol=crlf    # Force Windows line endings
*.jpg binary      # Mark as binary
```

---

## Next Steps

✅ **You now know how to control what Git tracks**

**Coming up:**

- Core Git concepts (repo, branch, commit, snapshot)
- Your first Git repository
- Basic Git commands

**Questions about .gitignore or .gitattributes?**
