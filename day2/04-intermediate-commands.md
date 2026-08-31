# Intermediate Commands: checkout, merge, pull, push, stash

_Essential commands for daily Git workflow_

---

### git checkout - Swiss Army Knife

#### Multiple uses:

- **Switch branches**
- **Restore files**
- **Create branches**
- **Checkout specific commits**

---

### git checkout: Branch Operations

#### Switch branches:

```bash
# Switch to existing branch
git checkout main
git checkout feature-branch

# Create and switch (older syntax)
git checkout -b new-feature

# Create branch from specific commit
git checkout -b hotfix abc123

# Switch to previous branch
git checkout -
```

#### Modern alternatives:

```bash
git switch main              # Instead of checkout
git switch -c new-feature    # Create and switch
git switch -                 # Previous branch
```

---

### git checkout: File Operations

#### Restore files:

```bash
# Discard changes in working directory
git checkout -- filename.txt
git checkout -- .

# Restore file from specific commit
git checkout abc123 -- filename.txt

# Restore file from different branch
git checkout feature-branch -- config.js
```

#### Modern alternative:

```bash
git restore filename.txt        # Discard changes
git restore --source=abc123 filename.txt  # From commit
```

---

### git merge - Combining Branches

#### Basic merge:

```bash
# Merge feature into current branch
git merge feature-branch

# Merge with custom message
git merge -m "Merge user authentication" feature-branch

# No fast-forward (always create merge commit)
git merge --no-ff feature-branch
```

#### Merge strategies:

```bash
# Prefer our changes in conflicts
git merge -X ours feature-branch

# Prefer their changes in conflicts
git merge -X theirs feature-branch

# Abort merge
git merge --abort
```

---

### git pull - Fetch and Merge

#### What git pull does:

```bash
git pull = git fetch + git merge
```

#### Basic usage:

```bash
# Pull from default remote/branch
git pull

# Pull from specific remote/branch
git pull origin main

# Pull with rebase instead of merge
git pull --rebase

# Pull all branches
git pull --all
```

---

### git pull Options

#### Rebase vs merge:

```bash
# Default: fetch + merge
git pull origin main

# Fetch + rebase (cleaner history)
git pull --rebase origin main

# Set as default
git config --global pull.rebase true
```

#### Fast-forward only:

```bash
# Only if fast-forward possible
git pull --ff-only origin main
```

---

### git push - Sharing Your Work

#### Basic push:

```bash
# Push to default remote/branch
git push

# Push specific branch
git push origin feature-branch

# Push and set upstream
git push -u origin feature-branch

# Push all branches
git push --all origin
```

#### Push variations:

```bash
# Push tags
git push origin --tags

# Push with commits and tags
git push --follow-tags

# Force push (dangerous!)
git push --force origin main
```

---

### git stash - Temporary Storage

#### What is stash?

**Temporarily save changes without committing**

#### When to use:

- **Switch branches** with uncommitted changes
- **Pull updates** with local modifications
- **Quick experiments** without losing work
- **Context switching**

---

### git stash Basic Operations

#### Save changes:

```bash
# Stash all changes
git stash

# Stash with message
git stash push -m "WIP: user authentication"

# Stash including untracked files
git stash -u

# Stash everything (including ignored files)
git stash -a
```

#### Restore changes:

```bash
# Apply most recent stash
git stash pop

# Apply specific stash
git stash apply stash@{2}

# Apply without removing from stash
git stash apply
```

---

### git stash Management

#### View stashes:

```bash
# List all stashes
git stash list

# Show stash contents
git stash show
git stash show -p  # With diff

# Show specific stash
git stash show stash@{1}
```

#### Clean up stashes:

```bash
# Delete specific stash
git stash drop stash@{1}

# Delete all stashes
git stash clear
```

---

### Practical Stash Workflow

#### Scenario: Need to switch branches

```bash
# You're working on feature A
echo "Work in progress" >> feature-a.js

# Urgent fix needed on main
git stash push -m "WIP: feature A implementation"

# Switch and fix
git switch main
# ... make urgent fix ...
git add .
git commit -m "fix: urgent production issue"

# Return to feature work
git switch feature-a
git stash pop
```

---

### Advanced Stash Operations

#### Partial stashing:

```bash
# Interactive stash
git stash -p

# Stash only staged changes
git stash --staged

# Stash specific files
git stash push -m "Config changes" config.js
```

#### Branch from stash:

```bash
# Create branch from stash
git stash branch new-feature stash@{1}
```

---

### Command Combinations

#### Common workflows:

```bash
# Update branch before feature work
git switch main
git pull origin main
git switch feature-branch
git merge main

# Save work and sync
git stash
git pull --rebase
git stash pop

# Quick branch switch
git stash
git switch other-branch
# ... do work ...
git switch -
git stash pop
```

---

### git checkout vs git switch/restore

#### Old way (git checkout):

```bash
git checkout main              # Switch branch
git checkout -b feature        # Create branch
git checkout -- file.txt      # Restore file
git checkout abc123            # Detached HEAD
```

#### New way (Git 2.23+):

```bash
git switch main                # Switch branch
git switch -c feature          # Create branch
git restore file.txt           # Restore file
git switch --detach abc123     # Detached HEAD
```

**Use new commands for clarity**

---

### Understanding HEAD

#### What is HEAD?

**Pointer to current branch/commit**

#### HEAD variations:

```bash
HEAD        # Current commit
HEAD~1      # Parent commit
HEAD~2      # Grandparent commit
HEAD^       # First parent (same as HEAD~1)
HEAD^2      # Second parent (merge commits)
```

#### Using HEAD:

```bash
git diff HEAD~1              # Compare with parent
git checkout HEAD~2          # Go back 2 commits
git reset HEAD~1             # Undo last commit
```

---

### Detached HEAD State

#### What is detached HEAD?

**HEAD points to specific commit, not a branch**

#### When it happens:

```bash
git checkout abc123          # Specific commit
git checkout v1.0.0          # Tag
git checkout HEAD~3          # Relative commit
```

#### Getting out:

```bash
# Create branch from current state
git switch -c new-branch

# Return to branch
git switch main
```

---

### Working with Remote Branches

#### Track remote branch:

```bash
# Create local branch tracking remote
git checkout -b local-name origin/remote-branch

# Or with switch
git switch -c local-name origin/remote-branch

# Track existing remote branch
git branch --set-upstream-to=origin/main main
```

#### Push new branch:

```bash
# Push and set tracking
git push -u origin new-feature

# Just push
git push origin new-feature
```

---

### Command Troubleshooting

#### Can't switch branches:

```bash
# Uncommitted changes
git status
git stash    # Save changes
git switch other-branch
git stash pop

# Or commit changes
git add .
git commit -m "WIP: save progress"
```

#### Push rejected:

```bash
# Remote has newer commits
git pull origin main
# Resolve conflicts if any
git push origin main
```

---

### Performance Tips

#### Shallow clones:

```bash
# Clone with limited history
git clone --depth 1 https://github.com/user/repo.git

# Unshallow later
git fetch --unshallow
```

#### Sparse checkout:

```bash
# Only checkout specific directories
git sparse-checkout init
git sparse-checkout set src/ docs/
```

---

### Best Practices

#### Pull regularly:

✅ **Pull before** starting work  
✅ **Pull before** pushing  
✅ **Use rebase** for cleaner history

#### Stash wisely:

✅ **Descriptive** stash messages  
✅ **Don't accumulate** too many stashes  
✅ **Clean up** old stashes

#### Branch management:

✅ **Delete** merged branches  
✅ **Keep** branch names descriptive  
✅ **Track** remote branches

---

### Questions?

**These commands form your daily Git toolkit**

**Coming up:** Hands-on practice with intermediate concepts

**Any questions about these intermediate commands?**
