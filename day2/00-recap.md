# Day 2: Recap & Troubleshooting From Day 1

_Reviewing core concepts and addressing questions_

---

## Welcome Back!

### Today's Focus:

- **Intermediate Git** concepts
- **Collaboration** workflows
- **Conflict resolution**
- **Version tracking** strategies

### But first... let's recap Day 1

---

## Day 1 Recap: What We Covered

### 1. What is Version Control?

- **Tracking changes** over time
- **Git vs other VCS** systems
- **Why Git won** the popularity contest

### 2. Installation & Configuration

- **Installing Git** on different platforms
- **Essential configuration** (user.name, user.email)
- **SSH keys** for authentication

---

## Day 1 Recap: Core Concepts

### 3. .gitignore & .gitattributes

- **Controlling** what Git tracks
- **Common ignore patterns**
- **Line ending** normalization

### 4. Fundamental Concepts

- **Repository** = Project + History
- **Commit** = Snapshot in time
- **Branch** = Pointer to commit
- **Three states**: Modified, Staged, Committed

---

## Day 1 Recap: Basic Commands

### 5. Essential Commands

```bash
git init          # Create repository
git clone         # Copy repository
git status        # Check current state
git add           # Stage changes
git diff          # Show changes
git commit        # Save snapshot
git log           # View history
```

---

## Day 1 Recap: Local vs Remote

### 6. Distributed Git

- **Local repository** = Complete, independent
- **Remote repository** = Shared, collaborative
- **Hosting platforms**: GitHub, GitLab, Bitbucket
- **SSH vs HTTPS** authentication

---

## Quick Knowledge Check 🧠

### Question 1:

**What's the difference between `git add .` and `git commit -m "message"`?**

**Answer:** `git add .` stages changes, `git commit` saves them permanently

<!-- .element: class="fragment" -->

### Question 2:

**Where is your complete Git history stored?**

**Answer:** In the `.git` folder in your repository

<!-- .element: class="fragment" -->

---

## Common Day 1 Confusion Points

### "Git is just backup"

❌ **Git is version control** with complete history, not just backup

### "Staging area is unnecessary"

❌ **Staging allows** selective commits and review before saving

### "I need internet for Git"

❌ **Git works offline** - only pushing/pulling needs internet

### "Commits are like saves"

❌ **Commits are snapshots** of entire project, not individual files

---

## Troubleshooting Common Issues

### Issue: "Please tell me who you are"

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

### Issue: "git: command not found"

- **Git not installed** or not in PATH
- **Restart terminal** after installation

### Issue: "Permission denied (publickey)"

- **SSH key** not set up correctly
- **Use HTTPS** instead, or fix SSH keys

---

## File States Review

```
Untracked → Modified → Staged → Committed
                          ↓          ↓         ↓
                       git add    git add   git commit

Working Dir  →  Staging  →  Repository
   (temp)       (ready)     (permanent)
```

### Commands for each transition:

- **Track**: `git add filename` (untracked → staged)
- **Stage**: `git add filename` (modified → staged)
- **Commit**: `git commit -m "message"` (staged → committed)

---

## Repository Structure Refresher

```
my-project/
├── .git/      # Git database (DON'T TOUCH!)
│   ├── objects/      # All commits, files
│   ├── refs/heads/   # Branch pointers
│   ├── HEAD          # Current branch
│   └── config        # Repository settings
├── .gitignore        # Ignore patterns
├── README.md         # Documentation
└── src/           # Your actual code
    ├── app.js
    └── utils.js
```

---

## Hands-on Review Exercise

**Let's refresh our memory with a quick exercise:**

1. **Check** current repository status
2. **Create** a new file
3. **Stage** the file
4. **Check** what will be committed
5. **Commit** with good message
6. **View** the history

```bash
git status
echo "Day 2 content" > day2.txt
git add day2.txt
git diff --staged
git commit -m "docs: add day 2 notes"
git log --oneline -3
```

---

## Yesterday's Questions Addressed

### "How do I undo staging?"

```bash
git restore --staged filename
# or older syntax:
git reset HEAD filename
```

### "How do I see what changed in a commit?"

```bash
git show abc123
# or
git diff abc123^..abc123
```

### "Can I change a commit message?"

```bash
git commit --amend -m "New message"
```

---

## Remote Repository Refresher

### Key concepts:

- **origin** = Default remote name
- **push** = Send commits to remote
- **pull** = Get commits from remote
- **clone** = Copy entire repository

### Common workflow:

```bash
git clone https://github.com/user/repo.git
# make changes
git add .
git commit -m "Add feature"
git push origin main
```

---

## Git Hosting Platforms Recap

### GitHub:

- **Largest community**
- **Free public repos**
- **Strong integrations**

### GitLab:

- **Built-in CI/CD**
- **Self-hosting option**
- **DevOps focused**

### Bitbucket:

- **Atlassian ecosystem**
- **Jira integration**
- **Enterprise features**

---

## What We'll Learn Today

### Morning (09:30-12:00):

- **Branching strategies** and workflows
- **Merging** and **merge conflicts**
- **Tagging** for releases

### Afternoon (13:00-17:00):

- **Intermediate commands**
- **Reset, revert, clean**
- **Recovery scenarios**
- **Advanced troubleshooting**

---

## Questions From Yesterday?

### Common topics:

- **Configuration** issues
- **SSH key** setup
- **Command syntax** clarification
- **Workflow** questions
- **Platform** comparisons

**Let's address any lingering questions before we move on...**

---

## Git Command Cheat Sheet

### Daily basics:

```bash
git status                    # What's happening?
git add .                     # Stage all changes
git commit -m "message"       # Save snapshot
git log --oneline            # View history
git diff                     # See unstaged changes
git diff --staged            # See staged changes
```

### Working with remotes:

```bash
git clone <url>              # Copy repository
git push origin main         # Send changes
git pull origin main         # Get changes
git remote -v                # Show remotes
```

---

## Mental Model Check

### Git Repository = ?

**Complete project + Full history**

<!-- .element: class="fragment" -->

### Commit = ?

**Snapshot of entire project at point in time**

<!-- .element: class="fragment" -->

### Branch = ?

**Movable pointer to specific commit**

<!-- .element: class="fragment" -->

### Remote = ?

**Reference to repository on another machine**

<!-- .element: class="fragment" -->

---

## Ready for Intermediate Git?

### You should now understand:

✅ **Basic Git workflow** (add, commit, push, pull)  
✅ **Repository structure** and the .git folder  
✅ **File states** (modified, staged, committed)  
✅ **Local vs remote** repositories  
✅ **Basic troubleshooting**

### Coming up:

🚀 **Branching and merging**  
🚀 **Conflict resolution**  
🚀 **Advanced Git commands**

---

## Let's Dive into Branching! 🌿

**Next:** [Core Concepts 3 - Branching Strategies & Merge](/day2/01-core-concepts-pt3.md)
