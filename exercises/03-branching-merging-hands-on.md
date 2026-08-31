# Hands-On Exercise: Branching & Merging

**Duration:** 30 minutes  
**Level:** Intermediate  
**Objective:** Master Git's branching and merging capabilities for feature development and collaboration

## Overview

In this exercise, you'll create and work with multiple branches, simulate a collaborative development workflow, and practice different merging strategies. This exercise builds upon the local and remote workflow skills from Day 1.

## Prerequisites

- Completed previous exercises (Local Workflow and Remotes)
- Understanding of basic Git commands
- Git repository with some commit history

## Learning Goals

By the end of this exercise, you will:

- Create and switch between branches confidently
- Understand different merge strategies
- Practice the feature branch workflow
- Handle fast-forward vs. three-way merges

## Exercise Steps

### Step 1: Prepare Your Working Repository (5 minutes)

1. **Create a new project or use existing one:**

   ```bash
   # Option A: Use existing project
   cd my-first-git-project

   # Option B: Create fresh project for this exercise
   mkdir branching-practice
   cd branching-practice
   git init
   ```

2. **If starting fresh, create initial content:**

   ```bash
   echo "# Branching Practice Project" > README.md
   echo "Learning Git branching and merging strategies" >> README.md

   echo "function calculateSum(a, b) {" > calculator.js
   echo "  return a + b;" >> calculator.js
   echo "}" >> calculator.js

   git add .
   git commit -m "Initial commit: basic calculator"
   ```

3. **Verify your starting point:**

   ```bash
   git log --oneline --graph
   git branch -v
   ```

### Step 2: Feature Branch Development (8 minutes)

1. **Create and switch to a feature branch:**

   ```bash
   git checkout -b feature/subtract-function
   # Or using modern syntax:
   # git switch -c feature/subtract-function
   ```

2. **Add the subtract functionality:**

   ```bash
   echo "" >> calculator.js
   echo "function calculateSubtract(a, b) {" >> calculator.js
   echo "  return a - b;" >> calculator.js
   echo "}" >> calculator.js
   ```

3. **Commit your changes:**

   ```bash
   git add calculator.js
   git commit -m "feat: add subtract function"
   ```

4. **Check your branch status:**

   ```bash
   git log --oneline --graph --all
   git branch -v
   ```

### Step 3: Parallel Development Simulation (8 minutes)

1. **Switch back to main and create another feature:**

   ```bash
   git checkout main
   git checkout -b feature/multiply-function
   ```

2. **Add multiply functionality:**

   ```bash
   echo "" >> calculator.js
   echo "function calculateMultiply(a, b) {" >> calculator.js
   echo "  return a * b;" >> calculator.js
   echo "}" >> calculator.js
   ```

3. **Commit the multiply feature:**

   ```bash
   git add calculator.js
   git commit -m "feat: add multiply function"
   ```

4. **Visualize your branch structure:**

   ```bash
   git log --oneline --graph --all --decorate
   ```

   **Expected Result:** You should see a branching structure with main and two feature branches.

### Step 4: Merging - Fast-Forward (4 minutes)

1. **Merge the subtract function (fast-forward merge):**

   ```bash
   git checkout main
   git merge feature/subtract-function
   ```

2. **Analyze what happened:**

   ```bash
   git log --oneline --graph
   ```

   **Question:** Why was this a fast-forward merge? What does that mean?

3. **Clean up the merged branch:**

   ```bash
   git branch -d feature/subtract-function
   ```

### Step 5: Merging - Three-Way Merge (5 minutes)

1. **Now merge the multiply function:**

   ```bash
   git merge feature/multiply-function
   ```

   **Note:** This should create a merge commit because main has moved forward.

2. **View the merge commit:**

   ```bash
   git log --oneline --graph --all
   git show HEAD
   ```

3. **Clean up:**

   ```bash
   git branch -d feature/multiply-function
   ```

## Challenge Tasks (Additional Practice)

If you finish early or want extra practice:

### Challenge 1: No Fast-Forward Merge

1. Create a new feature branch `feature/divide-function`
2. Add a divide function and commit it
3. Merge back to main using `git merge --no-ff feature/divide-function`
4. Compare the graph with your previous merges

### Challenge 2: Multiple Commits on Feature Branch

1. Create `feature/advanced-calculator`
2. Make 3 separate commits:
   - Add power function
   - Add square root function
   - Add validation for division by zero
3. Merge back and observe the commit history

## Verification & Reflection

### Verify Your Work

1. **Check final branch structure:**

   ```bash
   git branch -v
   git log --oneline --graph
   ```

2. **Verify your calculator.js contains all functions:**

   ```bash
   cat calculator.js
   ```

### Reflection Questions

1. **What's the difference between fast-forward and three-way merges?**

2. **When would you prefer `--no-ff` over the default merge behavior?**

3. **How does the commit graph help you understand project history?**

4. **What are the advantages of using feature branches?**

## Common Issues & Solutions

### Issue: "Already up to date" message

**Problem:** Git says "Already up to date" when trying to merge.

**Solution:** This means the branch you're merging has no new commits compared to your current branch. Check your branch status with `git log --graph --oneline --all`.

### Issue: Wrong branch during development

**Problem:** Made commits on the wrong branch.

**Solution:**

- Use `git log` to see where you are
- If commits are on main instead of feature branch: create branch from current position, reset main back

### Issue: Confused about current branch

**Problem:** Not sure which branch you're on.

**Solution:**

- `git branch` shows all branches with current marked by \*
- `git status` shows current branch in the first line
- Configure your shell prompt to show current branch

## Key Takeaways

- **Branches are cheap** - create them freely for features, experiments, fixes
- **Fast-forward merges** happen when target branch hasn't changed
- **Three-way merges** create explicit merge commits showing integration points
- **Feature branches** keep development organized and history clean
- **Graph visualization** helps understand project evolution and collaboration

## Next Steps

This exercise prepares you for the next hands-on session where you'll learn to handle merge conflicts and more advanced collaboration scenarios.
