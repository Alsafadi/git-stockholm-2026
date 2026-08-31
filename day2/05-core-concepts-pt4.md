# Core Concepts 4: Reset, Revert, Clean

_Mastering Git's undo and cleanup commands_

---

### Why We Need Undo Commands

#### Common scenarios:

- **Wrong commit** - Committed too early or to wrong branch
- **Bad changes** - Code that breaks functionality
- **Messy history** - Multiple small commits that should be one
- **Staging mistakes** - Added wrong files to staging area
- **Dirty working directory** - Untracked files cluttering workspace

#### Git's undo toolkit:

- **`git reset`** - Move branch pointer and optionally modify staging/working directory
- **`git revert`** - Create new commit that undoes previous commit
- **`git clean`** - Remove untracked files and directories
- **`git checkout`** - Restore files from specific commits
- **`git restore`** - Modern replacement for checkout file operations

---

### Understanding Git Reset

#### What `git reset` does:

**Moves the current branch pointer to a different commit**

#### The three trees Git manages:

```
HEAD (Branch pointer)
    ↓
Index (Staging area)
    ↓
Working Directory
```

#### Reset modes:

- **`--soft`** - Move HEAD only (keep staging and working directory)
- **`--mixed`** - Move HEAD and reset staging (default, keep working directory)
- **`--hard`** - Move HEAD, reset staging AND working directory

---

### Git Reset --soft

#### Moves HEAD, keeps staging and working directory:

```bash
# Before reset --soft
git log --oneline
a1b2c3d (HEAD -> main) Fix typo in documentation
d4e5f6g Add user authentication
h7i8j9k Initial commit

# Reset to previous commit but keep changes staged
git reset --soft HEAD~1

# After reset --soft
git log --oneline
d4e5f6g (HEAD -> main) Add user authentication
h7i8j9k Initial commit

git status
# Changes to be committed:
#   modified: docs/readme.md
```

#### Use cases:

✅ **Redo last commit** - Change commit message or add more files  
✅ **Combine commits** - Reset and recommit multiple changes together  
✅ **Move commits to different branch** - Reset then checkout and commit elsewhere

---

### Git Reset --mixed (Default)

#### Moves HEAD and unstages files:

```bash
# Before reset --mixed
git status
# On branch main
# nothing to commit, working tree clean

# Reset and unstage files
git reset HEAD~1
# Equivalent to: git reset --mixed HEAD~1

# After reset --mixed
git status
# Changes not staged for commit:
#   modified: docs/readme.md
#   modified: src/auth.js
```

#### Use cases:

✅ **Unstage files** - Remove files from staging area  
✅ **Redo staging** - Re-examine what should be committed  
✅ **Split large commit** - Break one commit into multiple smaller ones

---

### Git Reset --hard

#### ⚠️ **DESTRUCTIVE** - Loses working directory changes:

```bash
# Before reset --hard
git status
# Changes not staged for commit:
#   modified: src/main.js
#   modified: README.md

# Reset everything to previous commit
git reset --hard HEAD~1

# After reset --hard
git status
# On branch main
# nothing to commit, working tree clean

# ALL local changes are LOST!
```

#### Use cases:

✅ **Discard all changes** - Start fresh from specific commit  
✅ **Emergency cleanup** - Remove all uncommitted work  
⚠️ **Use with extreme caution** - Changes cannot be recovered easily

---

### Practical Reset Examples

#### Undo last commit but keep changes:

```bash
# Scenario: Committed too early, want to add more files
git log --oneline
abc123 (HEAD -> main) Add login feature

# Reset to keep changes for modification
git reset --soft HEAD~1

# Now add more files and recommit
git add forgotten-file.js
git commit -m "Add complete login feature with validation"
```

#### Unstage specific files:

```bash
# Accidentally staged wrong files
git add .
git status
# Changes to be committed:
#   new file: important.js
#   new file: temp.log
#   new file: debug.txt

# Unstage specific files
git reset HEAD temp.log debug.txt

# Or unstage everything
git reset HEAD
```

---

### Advanced Reset Scenarios

#### Reset to specific commit:

```bash
# View history
git log --oneline
e1f2g3h (HEAD -> main) Broken feature
d4e5f6g Working version
a1b2c3d Initial commit

# Reset to working version
git reset --hard d4e5f6g

# Now HEAD points to working version
git log --oneline
d4e5f6g (HEAD -> main) Working version
a1b2c3d Initial commit
```

#### Reset individual files:

```bash
# Reset specific file to previous version
git reset HEAD~1 -- src/config.js

# File is now unstaged with previous version
git status
# Changes to be committed:
#   modified: src/config.js
```

---

### Understanding Git Revert

#### What `git revert` does:

**Creates a new commit that undoes changes from a previous commit**

#### Key differences from reset:

- **Safe** - Doesn't change history
- **Additive** - Creates new commit instead of removing commits
- **Collaborative** - Safe to use on shared branches
- **Traceable** - Shows what was undone and why

#### Basic revert syntax:

```bash
# Revert the last commit
git revert HEAD

# Revert specific commit
git revert abc123

# Revert multiple commits
git revert HEAD~3..HEAD~1
```

---

### Git Revert Examples

#### Reverting a single commit:

```bash
# Before revert
git log --oneline
c1d2e3f (HEAD -> main) Bad feature that breaks things
a4b5c6d Good working code
x7y8z9w Initial commit

# Revert the bad commit
git revert c1d2e3f

# Git opens editor for revert commit message:
# Revert "Bad feature that breaks things"
#
# This reverts commit c1d2e3f because it caused system failures.

# After revert
git log --oneline
f9g8h7i (HEAD -> main) Revert "Bad feature that breaks things"
c1d2e3f Bad feature that breaks things
a4b5c6d Good working code
x7y8z9w Initial commit
```

---

### Revert vs Reset Comparison

#### When to use `git revert`:

- **Shared branches** - main, develop, release branches
- **Published commits** - Already pushed to remote
- **Team collaboration** - Others might have based work on these commits
- **Audit trail** - Want to show what was undone and why
- **Production fixes** - Safe way to undo problematic releases

#### When to use `git reset`:

- **Local branches** - Not yet shared with others
- **Private commits** - Not pushed to remote
- **Recent mistakes** - Just made the commit
- **Cleaning history** - Before sharing with team

---

### Advanced Revert Scenarios

#### Reverting merge commits:

```bash
# Merge commits have multiple parents
git log --oneline --graph
*   d1e2f3g (HEAD -> main) Merge feature branch
|\
| * a4b5c6d Feature commit 2
| * x7y8z9w Feature commit 1
|/
* h1i2j3k Main branch commit

# Revert merge commit (specify parent with -m)
git revert -m 1 d1e2f3g
# -m 1 means revert to first parent (main branch)
# -m 2 would mean revert to second parent (feature branch)
```

#### Revert without committing:

```bash
# Revert changes but don't commit yet
git revert --no-commit abc123

# Make additional changes then commit
git add modified-file.js
git commit -m "Revert problematic feature and add safeguards"
```

---

### Understanding Git Clean

#### What `git clean` does:

**Removes untracked files and directories from working directory**

#### Types of untracked content:

- **Untracked files** - New files not added to Git
- **Ignored files** - Files matching .gitignore patterns
- **Build artifacts** - Compiled files, logs, temp files
- **Empty directories** - Directories with no tracked files

#### Clean options:

- **`-n`** - Dry run (show what would be removed)
- **`-f`** - Force removal
- **`-d`** - Remove directories too
- **`-x`** - Remove ignored files too
- **`-X`** - Remove only ignored files

---

### Git Clean Examples

#### Safe cleaning workflow:

```bash
# 1. See what would be removed (dry run)
git clean -n
# Would remove temp.log
# Would remove debug.txt
# Would remove cache/

# 2. Remove untracked files
git clean -f

# 3. Remove untracked files AND directories
git clean -fd

# 4. Check status
git status
# On branch main
# nothing to commit, working tree clean
```

#### Clean specific patterns:

```bash
# Remove only .log files
git clean -f "*.log"

# Remove specific directory
git clean -fd temp/

# Interactive cleaning
git clean -i
# Would remove the following items:
#   cache/
#   temp.log
# *** Commands ***
#     1: clean    2: filter by pattern    3: select by numbers
#     4: ask each 5: quit                 6: help
# What now>
```

---

### Advanced Clean Operations

#### Clean ignored files:

```bash
# Remove files ignored by .gitignore
git clean -fx

# Common use case: clean build artifacts
# .gitignore contains:
# *.log
# node_modules/
# dist/
# .cache/

# Remove only ignored files (keep untracked)
git clean -fX

# Remove everything (tracked files remain)
git clean -fdx
```

#### Clean with exceptions:

```bash
# Clean but keep specific files
git clean -f -e "important.temp"

# Clean but keep files matching pattern
git clean -f -e "*.config"
```

---

### Combining Reset, Revert, and Clean

#### Complete workspace reset:

```bash
# Nuclear option: reset everything to last commit
git reset --hard HEAD
git clean -fdx

# Now workspace exactly matches last commit
git status
# On branch main
# nothing to commit, working tree clean
```

#### Selective cleanup:

```bash
# Unstage everything but keep changes
git reset HEAD

# Remove untracked files but keep modifications
git clean -fd

# Now review and stage only what you want
git add -p  # Interactive staging
```

---

### Recovery from Mistakes

#### Finding lost commits:

```bash
# View reflog to find lost commits
git reflog
# 8a9b0c1 HEAD@{0}: reset: moving to HEAD~1
# 2d3e4f5 HEAD@{1}: commit: Lost commit message
# 6g7h8i9 HEAD@{2}: commit: Previous commit

# Recover lost commit
git reset --hard 2d3e4f5

# Or create branch from lost commit
git branch recovery-branch 2d3e4f5
```

#### Recovering deleted files:

```bash
# File was deleted but not committed
git checkout HEAD -- deleted-file.txt

# File was deleted and committed
git show HEAD~1:path/to/deleted-file.txt > recovered-file.txt
```

---

### Best Practices

#### Before using destructive commands:

✅ **Create backup branch** - `git branch backup-$(date +%Y%m%d)`
✅ **Check git status** - Understand current state
✅ **Use dry run** - Test with `-n` flag when available
✅ **Start conservative** - Use `--soft` before `--hard`

#### Command safety levels:

🟢 **Safe** - `git revert`, `git clean -n`, `git reset --soft`
🟡 **Caution** - `git reset --mixed`, `git clean -f`
🔴 **Dangerous** - `git reset --hard`, `git clean -fdx`

#### Team collaboration:

✅ **Use revert on shared branches** - Never reset public history
✅ **Communicate destructive changes** - Warn team before force pushes
✅ **Document undo reasons** - Clear commit messages for reverts

---

### PowerShell Equivalents

#### Windows-specific considerations:

```powershell
# PowerShell aliases work the same
git reset --hard HEAD~1
git revert HEAD
git clean -fd

# Viewing files before cleaning
git clean -n | Select-String "Would remove"

# Remove files with specific extensions
git clean -f "*.tmp", "*.log"

# Interactive clean with PowerShell formatting
git clean -i
```

---

### Common Scenarios and Solutions

#### "I committed the wrong files":

```bash
# Solution 1: Soft reset and re-stage
git reset --soft HEAD~1
git add correct-files.js
git commit -m "Add correct implementation"

# Solution 2: Revert if already pushed
git revert HEAD
```

#### "My working directory is messy":

```bash
# Clean untracked files
git clean -fd

# Reset modified files
git checkout -- .

# Or use modern restore command
git restore .
```

#### "I want to undo last 3 commits":

```bash
# If not pushed (private branch)
git reset --hard HEAD~3

# If pushed (shared branch)
git revert HEAD~2..HEAD
```

---

### Summary

#### Reset modes comparison:

| Mode      | HEAD | Index | Workdir | Use Case           |
| --------- | ---- | ----- | ------- | ------------------ |
| `--soft`  | ✅   | ❌    | ❌      | Redo commit        |
| `--mixed` | ✅   | ✅    | ❌      | Unstage files      |
| `--hard`  | ✅   | ✅    | ✅      | Discard everything |

#### Command quick reference:

```bash
# Undo last commit, keep changes
git reset --soft HEAD~1

# Undo and unstage last commit
git reset HEAD~1

# Completely remove last commit
git reset --hard HEAD~1

# Safely undo published commit
git revert HEAD

# Clean untracked files
git clean -fd
```

---

### Questions?

**These commands are powerful tools for managing Git history**

**Next:** Hands-on practice with recovery scenarios

**Any questions about undoing changes or cleaning your workspace?**
