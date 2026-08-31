# Hands-On Exercise: Local Git Workflow

**Duration:** 30 minutes  
**Level:** Beginner/Intermediate  
**Objective:** Practice basic Git commands in a local repository workflow

## Overview

In this exercise, you'll create a local Git repository, make changes to files, and practice the fundamental Git commands that form the backbone of version control workflows.

## Prerequisites

- Git installed and configured on your system
- Basic command line knowledge
- Text editor of your choice

## Exercise Steps

### Step 1: Initialize a New Repository (5 minutes)

1. **Create a new project directory:**

   ```bash
   mkdir my-first-git-project
   cd my-first-git-project
   ```

2. **Initialize Git repository:**

   ```bash
   git init
   ```

3. **Check the status:**

   ```bash
   git status
   ```

   **Expected Output:** You should see that you're on the main/master branch with no commits yet.

### Step 2: Create and Track Your First File (8 minutes)

1. **Create a README file:**

   ```bash
   echo "# My First Git Project" > README.md
   echo "This is a practice repository for learning Git basics." >> README.md
   ```

2. **Check status again:**

   ```bash
   git status
   ```

   **Question:** What do you notice about the README.md file? Is it tracked or untracked?

3. **Add the file to staging area:**

   ```bash
   git add README.md
   ```

4. **Check status after adding:**

   ```bash
   git status
   ```

   **Question:** How has the status changed?

5. **View the differences:**

   ```bash
   git diff --cached
   ```

   **Note:** This shows changes in the staging area (cached changes).

6. **Make your first commit:**

   ```bash
   git commit -m "Initial commit: Add README file"
   ```

7. **Check status after commit:**
   ```bash
   git status
   ```

### Step 3: Make Changes and Practice Workflow (10 minutes)

1. **Create a new file:**

   ```bash
   echo "print('Hello, Git!')" > hello.py
   ```

2. **Modify the existing README:**

   ```bash
   echo "" >> README.md
   echo "## Files in this project:" >> README.md
   echo "- README.md: Project description" >> README.md
   echo "- hello.py: Simple Python script" >> README.md
   ```

3. **Check status:**

   ```bash
   git status
   ```

   **Question:** What files are shown and in what state?

4. **View differences for modified files:**

   ```bash
   git diff
   ```

   **Question:** What changes do you see for README.md?

5. **Add only one file:**

   ```bash
   git add hello.py
   ```

6. **Check status and compare diffs:**

   ```bash
   git status
   git diff
   git diff --cached
   ```

   **Question:** What's the difference between `git diff` and `git diff --cached`?

7. **Add the remaining file:**

   ```bash
   git add README.md
   ```

8. **Commit your changes:**
   ```bash
   git commit -m "Add Python script and update README"
   ```

### Step 4: Explore Git History (5 minutes)

1. **View commit history:**

   ```bash
   git log
   ```

2. **View condensed log:**

   ```bash
   git log --oneline
   ```

3. **View log with graph (even though it's simple):**

   ```bash
   git log --oneline --graph
   ```

4. **View specific commit details:**
   ```bash
   git log -1 --stat
   ```

### Step 5: Practice Clone Command (2 minutes)

1. **Navigate to parent directory:**

   ```bash
   cd ..
   ```

2. **Clone your local repository:**

   ```bash
   git clone my-first-git-project my-cloned-project
   ```

3. **Verify the clone:**
   ```bash
   cd my-cloned-project
   git log --oneline
   git status
   ```

## Reflection Questions

After completing the exercise, answer these questions:

1. **What is the difference between tracked and untracked files?**

2. **When would you use `git diff` vs `git diff --cached`?**

3. **What information does `git status` provide at each stage of the workflow?**

4. **Why is it important to write meaningful commit messages?**

5. **What's the difference between the working directory, staging area, and repository?**

## Bonus Challenges (If Time Permits)

1. **Create a `.gitignore` file:**

   ```bash
   echo "*.log" > .gitignore
   echo "__pycache__/" >> .gitignore
   touch debug.log
   git status
   ```

   **Observe:** How does Git handle the `.log` file?

2. **Practice amending commits:**

   ```bash
   echo "# Git Commands Practiced" >> README.md
   git add README.md
   git commit --amend -m "Add Python script, update README, and document Git commands"
   ```

3. **Explore different log formats:**
   ```bash
   git log --pretty=format:"%h - %an, %ar : %s"
   git log --since="1 hour ago"
   ```

## Key Takeaways

By the end of this exercise, you should understand:

- How to initialize a Git repository (`git init`)
- How to clone an existing repository (`git clone`)
- How to check repository status (`git status`)
- How to stage changes (`git add`)
- How to view differences (`git diff`)
- How to commit changes (`git commit`)
- How to view commit history (`git log`)
- The Git workflow: Working Directory → Staging Area → Repository

## Next Steps

In the next session, we'll explore:

- Working with remote repositories
- Branching and merging
- Resolving merge conflicts
- Collaborative workflows
