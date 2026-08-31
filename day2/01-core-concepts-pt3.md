# Core Concepts 3: Branching Strategies & Merge

_Mastering Git's killer feature: cheap and easy branching_

---

### Why Branching is Git's Superpower

#### In other VCS systems:

- **Branching is expensive** (copies entire codebase)
- **Merging is painful** (manual process)
- **Branches are rare** (used sparingly)

#### In Git:

- **Branching is instant** (just a pointer)
- **Merging is smart** (automatic when possible)
- **Branches are everywhere** (feature branches, bug fixes, experiments)

**Git was designed around branching**

---

### What is a Branch? (Deeper Dive)

#### A branch is simply:

- **A movable pointer** to a specific commit
- **41 bytes of data** (SHA-1 hash + newline)
- **A name** for a line of development

```
commit abc123 (HEAD -> main)
commit def456
commit ghi789
```

**main** points to abc123
**HEAD** points to main (current branch)

---

### Visualizing Branches

#### Linear development (no branches):

```
A --- B --- C --- D    (main branch)
                    ↑
                   HEAD
```

#### With feature branch:

```
A --- B --- C --- D    (main)
           \
            E --- F    (feature)
                  ↑
                 HEAD
```

**Shared history:** A, B, C
**Divergent development:** D vs E,F

---

### Creating Branches

#### Create new branch:

```bash
# Create branch (stays on current branch)
git branch feature-login

# Create and switch to branch
git checkout -b feature-login
# or newer syntax
git switch -c feature-login

# See all branches
git branch
```

#### What happens:

1. **New pointer** created at current commit
2. **Branch name** added to refs/heads/
3. **Working directory** unchanged (until switch)

---

### Switching Branches

#### Switch to existing branch:

```bash
# Older syntax
git checkout feature-login

# Newer syntax (Git 2.23+)
git switch feature-login

# Switch back to main
git switch main
```

#### What happens:

1. **HEAD** moves to target branch
2. **Working directory** updated to match
3. **Staging area** cleared

---

### Branch Workflow Demo

```bash
# Start on main
git switch main

# Create feature branch
git switch -c add-user-auth

# Make changes
echo "function login() {}" > auth.js
git add auth.js
git commit -m "feat: add login function"

# More changes
echo "function logout() {}" >> auth.js
git add auth.js
git commit -m "feat: add logout function"

# Check history
git log --oneline
```

---

### Branch States

#### Current state:

```
A --- B --- C           (main)
           \
            D --- E     (add-user-auth) ← HEAD
```

#### View branch status:

```bash
git branch -v    # Shows last commit on each branch
git log --graph --all --oneline  # Visual history
```

---

### Why Use Branches?

#### 1. **Feature Development**

- Isolate new features
- Experiment safely
- Keep main stable

#### 2. **Bug Fixes**

- Quick fixes without affecting features
- Test fixes independently
- Apply to multiple versions

#### 3. **Collaboration**

- Team members work independently
- Reduce conflicts
- Organized code review

---

### Common Branching Strategies

---

### Git Flow

#### Branch types:

- **main** - Production-ready code
- **develop** - Integration branch
- **feature/** - New features
- **release/** - Release preparation
- **hotfix/** - Emergency fixes

```
main:     A --- B --- C --- D
           \         /     /
develop:    E --- F --- G --
             \   /
feature:      H-I
```

**Good for:** Large teams, scheduled releases

---

### GitHub Flow

#### Simple strategy:

- **main** - Always deployable
- **feature branches** - All work happens here
- **Pull requests** - Code review and merge

```
main:     A --- B --- C --- E
           \           /
feature:    D ---------
```

**Good for:** Continuous deployment, small teams

---

### GitLab Flow

#### Environment branches:

- **main** - Development
- **staging** - Pre-production testing
- **production** - Live environment

```
main:        A --- B --- C --- D
              \               /
staging:       E --- F --- G
                \         /
production:      H --- I
```

**Good for:** Multiple environments, controlled releases

---

### What About Bitbucket?

Bitbucket doesn't brand its own named flow - teams using it typically pick
one of the strategies above and enforce it with **branch permissions** and
**merge checks** instead. Atlassian (Bitbucket's owner) is actually the
company that popularized the original **Git Flow** model shown a few slides
back, so that's a common default in Bitbucket shops.

---

### Feature Branch Workflow

#### 1. Create feature branch:

```bash
git switch main
git pull origin main
git switch -c feature/user-profile
```

#### 2. Develop feature:

```bash
# Make changes
git add .
git commit -m "feat: add user profile page"
# More commits...
```

#### 3. Push and create PR:

```bash
git push origin feature/user-profile
# Create pull request on GitHub
```

---

### Merging Branches

#### Two main types:

1. **Fast-forward merge** - Linear history
2. **Three-way merge** - Merge commit created

---

### Fast-Forward Merge

#### When possible:

```
Before merge:
main:     A --- B --- C
               \
feature:        D --- E

After merge:
main:     A --- B --- C --- D --- E
```

#### Command:

```bash
git switch main
git merge feature
```

**No merge commit** - just moves pointer forward

---

### Three-Way Merge

#### When branches diverged:

```
Before merge:
main:     A --- B --- C --- F
               \           /
feature:        D --- E ---

After merge:
main:     A --- B --- C --- F --- M
               \               /
feature:        D --- E -------
```

#### Command:

```bash
git switch main
git merge feature
```

**Creates merge commit** M with two parents

---

### Merge Command Options

#### Basic merge:

```bash
git merge feature-branch
```

#### No fast-forward (always create merge commit):

```bash
git merge --no-ff feature-branch
```

#### Fast-forward only (fail if not possible):

```bash
git merge --ff-only feature-branch
```

#### With custom message:

```bash
git merge -m "Merge user authentication feature" feature-branch
```

---

### Merge vs Rebase

#### Merge:

- **Preserves** branch history
- **Shows** when branches merged
- **Non-destructive**
- **Can be noisy** with many merges

#### Rebase:

- **Linear** history
- **Clean** timeline
- **Rewrites** history
- **Can be dangerous** on shared branches

**We'll cover rebase in detail on Day 2**

---

### Branch Management

#### List branches:

```bash
git branch         # Local branches
git branch -r      # Remote branches
git branch -a      # All branches
git branch -v      # With last commit
```

#### Delete branches:

```bash
git branch -d feature   # Safe delete (merged only)
git branch -D feature   # Force delete
git push origin --delete feature  # Delete remote branch
```

---

### Branch Naming Conventions

#### Good patterns:

```bash
feature/user-authentication
feature/add-search-function
bugfix/login-redirect-issue
hotfix/security-vulnerability
release/v1.2.0
```

#### Avoid:

```bash
my-branch
test
fix
new-stuff
branch1
```

**Use descriptive, consistent names**

---

### Remote Branches

#### Push new branch:

```bash
git push origin feature-branch
```

#### Set up tracking:

```bash
git push -u origin feature-branch
```

#### See remote branches:

```bash
git branch -r
git remote show origin
```

#### Track remote branch:

```bash
git switch -c local-name origin/remote-branch
```

---

### Merge Conflicts Preview

#### Conflicts occur when:

- **Same file** edited in both branches
- **Same lines** changed differently
- **Git can't** automatically resolve

#### Example conflict:

```
<<<<<<< HEAD
function greet() {
    return "Hello World!";
}
=======
function greet() {
    return "Hi there!";
}
>>>>>>> feature-branch
```

**We'll resolve conflicts in the next session**

---

### Best Practices

#### Branch creation:

✅ **Create from** updated main branch  
✅ **Use descriptive** names  
✅ **One feature** per branch  
✅ **Keep branches** short-lived

#### Merging:

✅ **Test before** merging  
✅ **Use pull requests** for review  
✅ **Delete** merged branches  
✅ **Update main** regularly

---

### Branching Anti-Patterns

❌ **Long-lived feature branches**

- Hard to merge
- Conflicts accumulate
- Features become stale

❌ **Working directly on main**

- No isolation
- Risky for collaboration
- No code review

❌ **Too many branches**

- Hard to track
- Maintenance overhead
- Context switching

---

### Demo: Complete Branch Workflow

Let's practice the full workflow:

```bash
# Start fresh
git switch main
git pull origin main

# Create feature branch
git switch -c feature/add-navbar

# Make changes
echo "<nav>Navigation</nav>" > navbar.html
git add navbar.html
git commit -m "feat: add navigation bar"

# Switch back and merge
git switch main
git merge feature/add-navbar

# Clean up
git branch -d feature/add-navbar
```

---

### Branch Visualization Tools

#### Command line:

```bash
git log --graph --oneline --all
git log --graph --pretty=format:'%h -%d %s (%cr) <%an>'
```

#### GUI tools:

- **GitKraken** - Visual branch timeline
- **SourceTree** - Atlassian's GUI
- **GitHub Desktop** - Simple branching
- **VS Code** - Built-in Git graph extensions

---

### When to Branch

#### Always branch for:

🌿 **New features**
🌿 **Bug fixes**\
🌿 **Experiments**
🌿 **Code reviews**

#### Maybe branch for:

🤔 **Documentation** updates  
🤔 **Small** configuration changes  
🤔 **Typo** fixes

#### Consider your team's workflow and complexity

---

### Troubleshooting Branches

#### Can't switch branches?

```bash
# Stash changes first
git stash
git switch other-branch
git stash pop
```

#### Lost branch?

```bash
# Find in reflog
git reflog
git checkout abc123  # Restore from commit hash
```

#### Accidentally on wrong branch?

```bash
git stash           # Save work
git switch correct-branch
git stash pop       # Restore work
```

---

### Questions?

**Branching is fundamental to modern Git workflows**

**Coming up:** Hands-on branching and merging practice

**Any questions about branches or merging strategies?**
