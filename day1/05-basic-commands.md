# Basic Commands: init, clone, status, add, diff, commit, log

_Essential Git commands for daily workflow_

---

## The Essential Git Workflow

```
1. Initialize or Clone repository
2. Make changes to files
3. Check status
4. Stage changes (add)
5. Review changes (diff)
6. Commit changes
7. View history (log)
```

**These 7 commands handle 90% of your Git usage**

---

### git init - Create New Repository

**Purpose:** Initialize a new Git repository

```bash
# Create new repository in current directory
git init

# Create new repository in specific directory
git init my-project

# Create bare repository (for servers)
git init --bare
```

#### What happens:

- Creates `.git` folder
- Sets up Git metadata
- Creates initial branch (usually `main`)
- Ready to track files

---

### git init Demo

```bash
# Create project directory
mkdir my-awesome-project
cd my-awesome-project

# Initialize Git repository
git init

# See what was created
ls -la
# Output: .git/ directory

# Check status
git status
# Output: "On branch main, No commits yet"
```

---

### git clone - Copy Remote Repository

**Purpose:** Download a repository from remote location

```bash
# Clone repository
git clone https://github.com/user/repo.git

# Clone to specific directory
git clone https://github.com/user/repo.git my-folder

# Clone specific branch
git clone -b feature-branch https://github.com/user/repo.git

# Clone with different protocols
git clone git@github.com:user/repo.git  # SSH
git clone https://github.com/user/repo.git  # HTTPS
```

---

### git clone vs git init

#### Use `git init` when:

- **Starting new project** from scratch
- **Converting existing** directory to Git repo
- **Creating local** repository

#### Use `git clone` when:

- **Working on existing** project
- **Contributing to** open source
- **Getting copy** of remote repository
- **Starting from** template repository

---

### git status - Check Repository State

**Purpose:** Show the working tree status

```bash
# Basic status
git status

# Short format
git status -s

# Show ignored files too
git status --ignored
```

#### Status shows:

- **Current branch**
- **Staged changes** (ready to commit)
- **Unstaged changes** (modified but not staged)
- **Untracked files** (new files)

---

### Understanding git status Output

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   README.md
        modified:   app.js

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   config.js

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        temp.log
```

---

### git add - Stage Changes

**Purpose:** Add file contents to the staging area

```bash
# Stage specific file
git add filename.txt

# Stage multiple files
git add file1.txt file2.txt

# Stage all changes in current directory
git add .

# Stage all changes in repository
git add -A

# Stage only modified/deleted files (not new files)
git add -u
```

---

### git add Variations

```bash
# Interactive staging
git add -i

# Patch mode - stage parts of files
git add -p

# Stage all .js files
git add *.js

# Stage files in specific directory
git add src/

# Stage by file pattern
git add "*.txt"
```

---

### git add Demo

```bash
# Create some files
echo "Hello World" > hello.txt
echo "console.log('Hi')" > app.js
mkdir src
echo "export default {}" > src/utils.js

# Check status
git status

# Stage one file
git add hello.txt

# Check status again
git status

# Stage remaining files
git add .
```

---

### git diff - Show Changes

**Purpose:** Show differences between commits, commit and working tree, etc.

```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged
# or
git diff --cached

# Show changes since specific commit
git diff HEAD~1

# Compare two commits
git diff abc123 def456
```

---

### git diff Output

```diff
diff --git a/app.js b/app.js
index 1234567..abcdefg 100644
--- a/app.js
+++ b/app.js
@@ -1,3 +1,4 @@
 function greet() {
-    console.log("Hello");
+    console.log("Hello World");
+    console.log("Welcome!");
 }
```

#### Reading diff:

- **--- a/app.js**: Original file
- **+++ b/app.js**: Modified file
- **-**: Lines removed (red)
- **+**: Lines added (green)

---

### git diff Use Cases

```bash
# Before staging - what changed?
git diff

# Before committing - what will be committed?
git diff --staged

# Compare with last commit
git diff HEAD

# Compare specific files
git diff app.js

# Word-level diff
git diff --word-diff

# Only show file names
git diff --name-only
```

---

### git commit - Save Changes

**Purpose:** Record changes to the repository

```bash
# Commit with message
git commit -m "Add user login feature"

# Commit with editor for longer message
git commit

# Commit and stage all modified files
git commit -am "Fix styling issues"

# Amend last commit
git commit --amend -m "Corrected commit message"
```

---

### git commit Best Practices

#### Good commit:

```bash
git commit -m "feat(auth): add password reset functionality

Users can now reset their passwords via email link.
Includes rate limiting to prevent abuse.

Fixes #123"
```

#### Poor commit:

```bash
git commit -m "stuff"
git commit -m "fixed it"
git commit -m "another update"
git commit -m "final FINAL Update"
```

---

### git log - View History

**Purpose:** Show commit logs

```bash
# Basic log
git log

# One line per commit
git log --oneline

# Show last 5 commits
git log -5

# Show commits with diff
git log -p

# Graphical representation
git log --graph --oneline --all
```

---

### git log Formatting

```bash
# Custom format
git log --pretty=format:"%h - %an, %ar : %s"

# Show commits by author
git log --author="John Doe"

# Show commits since date
git log --since="2 weeks ago"

# Show commits for specific file
git log -- filename.txt

# Show commits with specific message
git log --grep="fix"
```

---

### git log Output Examples

#### Default format:

```
commit abc123def456 (HEAD -> main)
Author: Jane Doe <jane@example.com>
Date:   Mon Oct 13 14:30:22 2025 +0100

    Add user authentication feature
```

#### Oneline format:

```
abc123d Add user authentication feature
def456a Fix login button styling
789ghi0 Update README documentation
```

---

### Putting It All Together - Complete Workflow

```bash
# 1. Start new project
git init my-project
cd my-project

# 2. Create initial files
echo "# My Project" > README.md
echo "console.log('Hello')" > app.js

# 3. Check what Git sees
git status

# 4. Stage files
git add .

# 5. Check what will be committed
git diff --staged

# 6. Make first commit
git commit -m "Initial commit: add README and app.js"

# 7. View history
git log --oneline
```

---

### Working with Existing Repository

```bash
# 1. Clone repository
git clone https://github.com/user/repo.git
cd repo

# 2. Make changes
echo "New feature" >> app.js

# 3. Check status
git status

# 4. See what changed
git diff

# 5. Stage changes
git add app.js

# 6. Commit changes
git commit -m "feat: add new feature functionality"

# 7. View updated history
git log --oneline -5
```

---

### Common Workflow Patterns

#### Quick commit everything:

```bash
git add .
git commit -m "Update all files"
```

#### Selective staging:

```bash
git add specific-file.js
git diff --staged
git commit -m "feat: update specific functionality"
```

#### Review before committing:

```bash
git diff
git add .
git diff --staged
git commit -m "fix: resolve validation issues"
```

---

### Undoing Common Mistakes

#### Unstage file:

```bash
git restore --staged filename.txt
# or older syntax
git reset HEAD filename.txt
```

#### Discard unstaged changes:

```bash
git restore filename.txt
# or older syntax
git checkout -- filename.txt
```

#### Amend last commit:

```bash
git commit --amend -m "Corrected commit message"
```

---

### Command Aliases

**Make your life easier with shortcuts:**

```bash
# Set up common aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'

# Now you can use:
git st        # instead of git status
git ci -m     # instead of git commit -m
git unstage   # instead of git reset HEAD
```

---

### Command Summary

| Command      | Purpose           | Common Usage          |
| ------------ | ----------------- | --------------------- |
| `git init`   | Create repository | `git init`            |
| `git clone`  | Copy repository   | `git clone <url>`     |
| `git status` | Check status      | `git status`          |
| `git add`    | Stage changes     | `git add .`           |
| `git diff`   | Show changes      | `git diff --staged`   |
| `git commit` | Save changes      | `git commit -m "msg"` |
| `git log`    | View history      | `git log --oneline`   |

---

### Practice Exercise

**Let's practice together:**

1. **Create** a new repository
2. **Add** some files (README, code file)
3. **Stage** changes step by step
4. **Review** changes with diff
5. **Commit** with good message
6. **Make** more changes
7. **Repeat** the cycle
8. **View** project history

---

### Common Beginner Mistakes

#### ❌ Forgetting to stage:

```bash
git commit -m "Add new feature"
# Error: nothing to commit
```

#### ✅ Remember to stage:

```bash
git add .
git commit -m "Add new feature"
```

#### ❌ Committing too much:

```bash
git add .  # Stages everything including temp files
```

#### ✅ Be selective:

```bash
git add src/  # Only stage source files
```

---

### Tips for Success

#### 📝 Always check status:

```bash
git status  # Before and after every command
```

#### 👀 Review before committing:

```bash
git diff --staged  # See what you're about to commit
```

#### 📦 Commit frequently:

- **Small, logical changes**
- **Working features**
- **Clear, descriptive messages**

#### 📚 Use help:

```bash
git help <command>  # Detailed documentation
git <command> --help  # Same thing
```

---

## Questions?

**These commands form the foundation of your Git workflow**

**Coming up after break:**

- Hands-on practice session
- Local workflow exercises
- Working with remote repositories

**Any questions about these basic commands?**
