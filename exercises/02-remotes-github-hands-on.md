# Hands-On Exercise: Working with Remotes and GitHub

**Duration:** 45 minutes  
**Level:** Beginner/Intermediate  
**Objective:** Practice working with remote repositories, specifically GitHub, and understand the local vs remote workflow

## Overview

In this exercise, you'll learn how to connect your local Git repository to a remote repository on GitHub, push your changes, and understand the fundamental concepts of distributed version control.

## Prerequisites

- Completed the Local Workflow hands-on exercise
- GitHub account created
- Git configured with your username and email
- Basic command line knowledge

## Before We Start

**Important:** Make sure you have your GitHub credentials ready. You'll need either:

- Personal Access Token (recommended)
- SSH key configured
- GitHub CLI authenticated

## Exercise Steps

### Step 1: Prepare Your Local Repository (5 minutes)

1. **Navigate to your previous project or create a new one:**

   ```bash
   cd my-first-git-project
   # OR create a new one if needed:
   # mkdir github-practice && cd github-practice && git init
   ```

2. **Verify you have commits:**

   ```bash
   git log --oneline
   ```

3. **Add more content to make it interesting:**

   ```bash
   echo "## About This Project" >> README.md
   echo "This project demonstrates Git and GitHub integration." >> README.md
   echo "Created during Git training course." >> README.md
   ```

4. **Create a simple project structure:**

   ```bash
   mkdir src
   echo "# Git Commands Reference" > docs.md
   echo "- git init: Initialize repository" >> docs.md
   echo "- git add: Stage changes" >> docs.md
   echo "- git commit: Save changes" >> docs.md
   echo "- git push: Upload to remote" >> docs.md
   ```

5. **Stage and commit these changes:**
   ```bash
   git add .
   git commit -m "Enhance project structure and documentation"
   ```

### Step 2: Create a GitHub Repository (8 minutes)

1. **Go to GitHub.com and create a new repository:**

   - Click "New" or "+" → "New repository"
   - Repository name: `git-course-practice` (or similar)
   - Description: `Practice repository for Git course`
   - Keep it **Public** for this exercise
   - **DO NOT** initialize with README, .gitignore, or license (we already have content)
   - Click "Create repository"

2. **Copy the repository URL:**

   - You'll see setup instructions
   - Copy the HTTPS URL (should look like: `https://github.com/<yourusername>/git-course-practice.git`)

3. **Note the commands GitHub suggests:**
   - GitHub provides commands for different scenarios
   - We'll use the "push an existing repository" commands

### Step 3: Connect Local Repository to Remote (10 minutes)

1. **Add the remote origin:**

   ```bash
   git remote add origin https://github.com/<yourusername>/git-course-practice.git
   ```

   **Replace `<yourusername>`** with your actual GitHub username!

2. **Verify the remote was added:**

   ```bash
   git remote -v
   ```

   **Expected Output:** You should see both fetch and push URLs for origin.

3. **Check the status:**

   ```bash
   git status
   ```

   **Question:** What does the status tell you about your branch's relationship to the remote?

4. **Push your code to GitHub:**

   ```bash
   git push -u origin main
   ```

   **Note:**

   - `-u` sets upstream tracking
   - You might be prompted for credentials
   - If your default branch is `master`, use `master` instead of `main`

5. **If you get an authentication error:**

   - For HTTPS: You'll need a Personal Access Token
   - For SSH: You'll need SSH keys configured
   - Ask instructor for help if needed

6. **Verify the push worked:**
   - Go to your GitHub repository page
   - Refresh if needed
   - You should see your files and commits

### Step 4: Make Changes and Practice Remote Workflow (15 minutes)

1. **Create a new feature:**

   ```bash
   echo "def greet(name):" > src/greeting.py
   echo "    return f'Hello, {name}! Welcome to Git!'" >> src/greeting.py
   echo "" >> src/greeting.py
   echo "if __name__ == '__main__':" >> src/greeting.py
   echo "    print(greet('Developer'))" >> src/greeting.py
   ```

2. **Add to documentation:**

   ```bash
   echo "- git remote: Manage remote repositories" >> docs.md
   echo "- git push: Upload changes to remote" >> docs.md
   echo "- git pull: Download changes from remote" >> docs.md
   ```

3. **Stage and commit locally:**

   ```bash
   git add .
   git commit -m "Add greeting function and update documentation"
   ```

4. **Check status before pushing:**

   ```bash
   git status
   ```

   **Question:** What does Git tell you about your local branch compared to the remote?

5. **Push the changes:**

   ```bash
   git push
   ```

   **Note:** Since we set upstream tracking, we don't need `-u origin main` anymore.

6. **Verify on GitHub:**
   - Refresh your GitHub repository page
   - Check that your new files and commits appear
   - Click on commits to see the history

### Step 5: Understanding Remote Information (5 minutes)

1. **Check remote information:**

   ```bash
   git remote show origin
   ```

   **Question:** What information does this command provide?

2. **View all branches (local and remote):**

   ```bash
   git branch -a
   ```

3. **Check log with remote information:**

   ```bash
   git log --oneline --graph --all
   ```

4. **See what files exist on remote:**
   ```bash
   git ls-remote origin
   ```

### Step 6: Simulate Collaboration (Bonus - 2 minutes)

1. **Make a change directly on GitHub:**

   - Go to your repository on GitHub
   - Click on `README.md`
   - Click the edit button (pencil icon)
   - Add a line: `This line was added directly on GitHub!`
   - Scroll down and commit the change

2. **Back in your terminal, check status:**

   ```bash
   git status
   ```

   **Question:** Does Git know about the change you made on GitHub?

3. **Fetch the remote changes:**

   ```bash
   git fetch origin
   ```

4. **Check status again:**

   ```bash
   git status
   ```

   **Question:** What does Git tell you now?

5. **Pull the changes:**

   ```bash
   git pull
   ```

6. **Verify the change:**
   ```bash
   cat README.md
   ```

## Reflection Questions

After completing the exercise, answer these questions:

1. **What is the difference between a local and remote repository?**

2. **What does `git remote add origin <url>` accomplish?**

3. **What's the difference between `git fetch` and `git pull`?**

4. **Why do we use `git push -u origin main` for the first push?**

5. **How can you tell if your local branch is ahead of, behind, or in sync with the remote?**

6. **What are the advantages of using a hosted platform like GitHub?**

## Common Issues and Solutions

### Authentication Problems

- **HTTPS:** Use Personal Access Token instead of password
- **SSH:** Ensure SSH keys are properly configured
- **Two-Factor Authentication:** Must use token or SSH

### Branch Name Differences

- Some repositories use `main`, others use `master`
- Check with: `git branch` and `git remote show origin`
- GitHub is transitioning to `main` as default

### Push Rejected

- Usually means remote has changes you don't have locally
- Solution: `git pull` first, then `git push`

## Key Takeaways

By the end of this exercise, you should understand:

- How to create a repository on GitHub
- How to connect local repository to remote (`git remote add`)
- How to push changes to remote repository (`git push`)
- How to check remote repository information (`git remote show`)
- The difference between local and remote branches
- Basic collaboration workflow with `git fetch` and `git pull`
- The concept of upstream tracking

## Next Steps

In the next sessions, we'll explore:

- Branching and merging strategies
- Handling merge conflicts
- Collaborative workflows with multiple contributors
- Using Git GUIs and clients
