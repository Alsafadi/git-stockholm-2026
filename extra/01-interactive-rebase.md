# Interactive Rebase: Squash, Reword, Reordering

_Rewriting history to tell a better story_

---

testing

### What is Interactive Rebase?

#### Definition:

**A powerful tool to modify commit history by replaying commits with changes**

#### What you can do:

- **Squash** multiple commits into one
- **Reword** commit messages
- **Reorder** commits
- **Edit** commit content
- **Drop** unwanted commits
- **Split** commits

#### ⚠️ **Warning:** Never rebase shared/public branches!

---

### When to Use Interactive Rebase

#### Good scenarios:

✅ **Clean up** feature branch before merge  
✅ **Fix** typos in commit messages  
✅ **Combine** related commits  
✅ **Remove** debugging commits  
✅ **Reorder** logical sequence

#### Avoid on:

❌ **Shared branches** (main, develop)  
❌ **Public commits** (already pushed)  
❌ **Other people's** work  
❌ **Complex merge** commits

---

### Basic Interactive Rebase

#### Start interactive rebase:

```bash
# Rebase last 3 commits
git rebase -i HEAD~3

# Rebase since specific commit
git rebase -i abc123

# Rebase since beginning
git rebase -i --root
```

#### What happens:

1. **Editor opens** with commit list
2. **Choose actions** for each commit
3. **Git replays** commits with modifications

---

### The Rebase Editor

#### Example rebase file:

```
pick abc123 feat: add user authentication
pick def456 fix: typo in login function
pick ghi789 feat: add logout functionality
pick jkl012 fix: another typo

# Rebase commands:
# p, pick = use commit
# r, reword = use commit, but edit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous
# f, fixup = like squash, but discard message
# d, drop = remove commit
```

---

### Rebase Commands Explained

#### pick (p):

**Use the commit as-is**

```
pick abc123 feat: **add** user authentication
```

#### reword (r):

**Change commit message**

```
reword def456 fix: typo in login function
```

#### squash (s):

**Combine with previous commit, keep both messages**

```
pick abc123 feat: add user authentication
squash def456 fix: typo in login function
```

---

### More Rebase Commands

#### fixup (f):

**Combine with previous, discard this message**

```
pick abc123 feat: add user authentication
fixup def456 fix: typo in login function
```

#### edit (e):

**Stop to modify commit**

```
edit abc123 feat: add user authentication
```

#### drop (d):

**Remove commit entirely**

```
drop def456 fix: debugging code
```

---

### Practical Example: Cleaning Feature Branch

#### Original messy history:

```bash
git log --oneline
ghi789 feat: add logout functionality
def456 fix: typo in login
abc123 feat: add login functionality
xyz999 fix: another typo
```

#### Target: Clean, logical commits

```bash
git rebase -i HEAD~4
```

#### In editor:

```
pick abc123 feat: add login functionality
fixup def456 fix: typo in login
pick ghi789 feat: add logout functionality
drop xyz999 fix: another typo
```

---

### Squashing Commits

#### Before squash:

```
commit c: fix: remove debug logs
commit b: fix: handle edge case
commit a: feat: add user registration
```

#### Rebase plan:

```
pick a: feat: add user registration
squash b: fix: handle edge case
squash c: fix: remove debug logs
```

#### Result:

```
commit: feat: add user registration

- Add user registration form
- Handle edge case for invalid emails
- Remove debug logs
```

---

### Reordering Commits

#### Original order:

```
pick def456 docs: update README
pick abc123 feat: add authentication
pick ghi789 test: add auth tests
```

#### Logical order:

```
pick abc123 feat: add authentication
pick ghi789 test: add auth tests
pick def456 docs: update README
```

**Git will replay commits in new order**

---

### Editing Commits

#### When you choose 'edit':

```bash
# Git stops at the commit
# Make your changes
echo "Additional content" >> file.txt
git add file.txt

# Continue or amend
git commit --amend
git rebase --continue
```

#### Use cases:

- **Add forgotten** file
- **Remove** sensitive data
- **Split** large commit
- **Fix** code issues

---

### Splitting Commits

#### If commit is too large:

```bash
# Start rebase and choose 'edit'
git rebase -i HEAD~3

# Reset the commit but keep changes
git reset HEAD^

# Stage and commit in parts
git add auth.js
git commit -m "feat: add authentication logic"

git add auth.test.js
git commit -m "test: add authentication tests"

# Continue rebase
git rebase --continue
```

---

### Handling Conflicts During Rebase

#### When conflicts occur:

```bash
# Fix conflicts in files
# Stage resolved files
git add conflicted-file.js

# Continue rebase
git rebase --continue

# Or abort if too complex
git rebase --abort
```

#### Tips:

- **Resolve** conflicts step by step
- **Test** after each resolution
- **Abort** if it gets too complex

---

### Advanced Rebase Options

#### Preserve merge commits:

```bash
git rebase -i --preserve-merges HEAD~5
```

#### Rebase onto different branch:

```bash
git rebase -i --onto main feature-base feature-branch
```

#### Sign rebased commits:

```bash
git rebase -i --gpg-sign HEAD~3
```

---

### Rebase vs Merge

#### Rebase creates linear history:

```
Before:
main:     A --- B --- C
               \       \
feature:        D --- E --- F

After rebase:
main:     A --- B --- C --- D' --- E' --- F'
```

#### Merge preserves branch structure:

```
main:     A --- B --- C -------- M
               \               /
feature:        D --- E --- F --
```

---

### When NOT to Rebase

#### ❌ Never rebase:

- **Public commits** (already pushed and shared)
- **Main/master** branch
- **Shared feature** branches
- **Release** branches
- **When unsure** about consequences

#### ✅ Safe to rebase:

- **Local commits** not yet pushed
- **Personal feature** branches
- **Draft commits** and experiments

---

### Collaborative Rebase Workflow

#### Safe approach:

```bash
# Work on feature branch
git switch feature/user-auth

# Make messy commits locally
git commit -m "WIP: auth stuff"
git commit -m "fix typo"
git commit -m "actually fix it"

# Clean up before sharing
git rebase -i HEAD~3

# Now push clean history
git push origin feature/user-auth
```

---

### Rebase Best Practices

#### Preparation:

✅ **Backup** your branch first  
✅ **Clean working** directory  
✅ **Understand** what you're changing  
✅ **Start small** - practice on copies

#### During rebase:

✅ **Read instructions** carefully  
✅ **Test** after conflicts  
✅ **Commit meaningful** chunks  
✅ **Write good** commit messages

---

### Emergency Rebase Recovery

#### If rebase goes wrong:

```bash
# Find previous state
git reflog

# Reset to before rebase
git reset --hard HEAD@{5}  # or whatever reflog shows

# Or cherry-pick specific commits
git cherry-pick abc123 def456
```

#### Backup strategy:

```bash
# Create backup branch before rebase
git branch backup-feature
git rebase -i HEAD~5
```

---

### Interactive Rebase Shortcuts

#### Common patterns:

```bash
# Squash last 3 commits
git reset --soft HEAD~3
git commit -m "Combined feature implementation"

# Fixup last commit
git commit --fixup HEAD
git rebase -i --autosquash HEAD~2

# Auto-squash marked commits
git rebase -i --autosquash HEAD~5
```

---

### Tools for Interactive Rebase

#### Command line editors:

- **Vim/Nano** - Built-in
- **VS Code** - `git config --global core.editor "code --wait"`

#### GUI tools:

- **GitKraken** - Visual rebase
- **SourceTree** - Interactive rebase
- **GitHub Desktop** - Limited support

#### IDE integration:

- **VS Code** - GitLens extension
- **IntelliJ** - Built-in Git tools

---

### Demo: Complete Rebase Workflow

Let's practice interactive rebase:

```bash
# Create test commits
git commit -m "feat: add login"
git commit -m "fix: typo"
git commit -m "feat: add logout"
git commit -m "fix: remove console.log"

# Interactive rebase
git rebase -i HEAD~4

# Clean up:
# - Squash fixes into features
# - Reword unclear messages
# - Drop debugging commits
```

---

### Questions About Interactive Rebase?

**Interactive rebase is powerful but requires practice**

**Coming up:** Hands-on rebase workflows

**Any questions about rewriting commit history?**
