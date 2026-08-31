# Core Concepts 1: Repo, Branch, Commit, Snapshot

_Understanding Git's fundamental building blocks_

---

## Git's Core Philosophy

### Git thinks about data as...

**Snapshots, not differences**

- Other VCS: Store changes/deltas
- Git: Store complete snapshots
- Efficient through object deduplication

---

## What is a Repository?

A **repository** (or "repo") is a directory that Git tracks

### Contains:

- **Your project files** (working directory)
- **Git metadata** (`.git` folder)
- **Complete history** of all changes
- **Branches, tags, configuration**

### Two types:

- **Local repository** (on your machine)
- **Remote repository** (on GitHub/GitLab/etc.)

---

## Repository Structure

```
my-project/
├── .git/                 # Git's metadata (don't touch!)
│   ├── objects/         # All commits, trees, blobs
│   ├── refs/            # Branch and tag references
│   ├── HEAD             # Current branch pointer
│   └── config           # Repository configuration
├── .gitignore           # Files to ignore
├── README.md            # Project documentation
└── src/                 # Your actual project files
    └── app.js
```

**The `.git` folder IS your repository's memory**

---

## Three States of Files

Files in Git exist in three states:

### 1. Modified

- Changed but not staged
- In your working directory

### 2. Staged

- Modified and marked for next commit
- In the staging area

### 3. Committed

- Safely stored in Git database
- Part of repository history

---

## The Three Areas

```
Working Directory    Staging Area      Git Repository
     (modified)       (staged)         (committed)

    [app.js*]   →   [app.js]    →    [commit abc123]
                      ready           permanent
                                      snapshot
```

### Workflow:

1. **Modify** files in working directory
2. **Stage** changes you want to commit
3. **Commit** staged changes to repository

---

## What is a Commit?

A **commit** is a snapshot of your project at a specific point in time

### Each commit contains:

- **Snapshot** of all tracked files
- **Author** information (name, email)
- **Timestamp** (when it was created)
- **Commit message** (what changed)
- **Parent commit(s)** (what came before)
- **Unique hash** (SHA-1 identifier)

---

## Commit Structure

```
commit abc123def456...
Author: Jane Doe <jane@example.com>
Date: Mon Oct 13 14:30:22 2025 +0100

    Add user authentication feature

    - Implement login/logout functionality
    - Add password hashing
    - Create user session management
```

### The hash:

- `abc123def456...` (40 characters)
- **Unique identifier** for this exact commit
- **Content-based** - same content = same hash

---

## Commits Form a History

```
A --- B --- C --- D    (main branch)
                              ↑
                             HEAD
```

Each commit points to its parent:

- **A**: Initial commit (no parent)
- **B**: Points to A
- **C**: Points to B
- **D**: Points to C (current)

**HEAD**: Points to current commit

---

## What is a Branch?

A **branch** is a movable pointer to a specific commit

### Key concepts:

- **Lightweight** - just a pointer to a commit
- **Independent** - changes don't affect other branches
- **Mergeable** - can combine branches later
- **Default branch** - usually called `main` or `master`

---

## Visualizing Branches

```
A --- B --- C --- D    (main)
                        \
                          E --- F    (feature)
                                ↑
                               HEAD
```

### What we see:

- **main branch**: Points to commit D
- **feature branch**: Points to commit F
- **HEAD**: Currently on feature branch
- **Shared history**: A, B, C are common

---

## Why Use Branches?

### Scenarios:

- **Feature development** - New functionality
- **Bug fixes** - Isolate fix from main code
- **Experiments** - Try something risky
- **Collaboration** - Team members work separately
- **Releases** - Maintain stable versions

### Benefits:

- **Safe experimentation**
- **Parallel development**
- **Easy context switching**
- **Clean history**

---

## Snapshots vs. Deltas

### Other VCS (Delta-based):

```
Version 1: File A, File B, File C
Version 2: File A, File B changed, File C
Version 3: File A changed, File B changed, File C
```

### Git (Snapshot-based):

```
Version 1: [A1][B1][C1]
Version 2: [A1][B2][C1]  ← B2 is new, A1/C1 are links
Version 3: [A2][B2][C1]  ← A2 is new, B2/C1 are links
```

**Git stores full snapshots but efficiently reuses unchanged files**

---

### Git Objects

Git stores everything as objects:

#### 1. Blob (Binary Large Object)

- **Content**: File contents
- **Name**: SHA-1 hash of content
- **Immutable**: Same content = same hash

#### 2. Tree

- **Content**: Directory listing (files + subdirs)
- **Points to**: Blobs and other trees
- **Like**: Filesystem snapshot

#### 3. Commit

- **Content**: Metadata + pointer to root tree
- **Points to**: Tree object + parent commit(s)

---

## Object Relationships

```
Commit Object
├── tree: 48f7a1b...     (root directory)
├── parent: d3e5f7g...   (previous commit)
├── author: Jane Doe
├── date: 2025-10-13
└── message: "Add new feature"

Tree Object (48f7a1b...)
├── blob: 1a2b3c4... README.md
├── blob: 5d6e7f8... app.js
└── tree: 9g0h1i2... src/

Blob Object (1a2b3c4...)
└── content: "# My Project\n\nThis is..."
```

---

## Working Directory vs Repository

### Working Directory:

- **What you see** in file explorer
- **Current state** of files
- **Can be modified** freely
- **Not versioned** until committed

### Repository (.git folder):

- **Git's database** of all history
- **Immutable snapshots**
- **All branches and commits**
- **Complete project timeline**

---

## The Staging Area (Index)

**The staging area is where you prepare your next commit**

### Purpose:

- **Review changes** before committing
- **Partial commits** - stage only some changes
- **Organize commits** - logical groupings
- **Quality control** - final check before saving

### Think of it as:

- **Shopping cart** before checkout
- **Rough draft** before final version
- **Rehearsal** before performance

---

## Demo: Creating Your First Repository

Let's see these concepts in action:

```bash
# Create new repository
mkdir my-project
cd my-project
git init

# Check status
git status

# Create a file
echo "Hello Git!" > hello.txt

# See Git's perspective
git status
```

**What do you notice?**

---

## Demo: Making Your First Commit

```bash
# Stage the file
git add hello.txt

# Check status again
git status

# Make the commit
git commit -m "Initial commit: add hello.txt"

# See the commit
git log

# Check status
git status
```

**What changed?**

---

## Understanding HEAD

**HEAD** is Git's way of saying "you are here"

### HEAD points to:

- **Current branch** (usually)
- **Specific commit** (detached HEAD)

```bash
# See where HEAD points
cat .git/HEAD

# See what HEAD points to
git log --oneline -1

# See all references
git show-ref
```

---

## Practical Exercise

Create a simple repository and explore:

1. **Initialize** repository
2. **Create** some files
3. **Stage** changes
4. **Make** commits
5. **Check** status at each step
6. **View** history with `git log`

**Let's do this together!**

---

## Common Misconceptions

### ❌ "Git is just backup"

**✅** Git is a complete project time machine

### ❌ "Commits are like file saves"

**✅** Commits are complete project snapshots

### ❌ "Branches are copies of files"

**✅** Branches are just pointers to commits

### ❌ "Git stores differences"

**✅** Git stores complete snapshots (efficiently)

---

## Key Takeaways

### Repository:

- **Complete project** + **full history**
- **Local** and **remote** copies

### Commit:

- **Immutable snapshot** of entire project
- **Unique hash** identifies content
- **Forms chain** of project history

### Branch:

- **Movable pointer** to specific commit
- **Enables parallel** development
- **Lightweight** and **fast**

### Snapshot:

- **Complete project state** at point in time
- **Efficient storage** through deduplication

---

## Questions?

**Understanding these core concepts is crucial for everything else we'll learn**

**Next up:** Commit Messages & Conventions

---
