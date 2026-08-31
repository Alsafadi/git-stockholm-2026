# Hands-On Exercise: Merge Conflicts & Resolution

**Duration:** 30 minutes  
**Level:** Intermediate  
**Objective:** Learn to identify, understand, and resolve merge conflicts using both command line and GUI tools

## Overview

In this exercise, you'll intentionally create merge conflicts and practice resolving them. Merge conflicts are a normal part of collaborative development, and knowing how to handle them confidently is essential for any Git user.

## Prerequisites

- Completed branching and merging exercise
- Basic understanding of Git branching and merging
- Text editor or IDE available

## Learning Goals

By the end of this exercise, you will:

- Understand when and why merge conflicts occur
- Read and interpret Git's conflict markers
- Resolve conflicts manually using a text editor
- Use Git tools to help with conflict resolution
- Practice different conflict resolution strategies

## Exercise Steps

### Step 1: Setup Conflict Scenario (5 minutes)

1. **Create a new project for practicing conflicts:**

   ```bash
   mkdir conflict-practice
   cd conflict-practice
   git init
   ```

2. **Create a file that we'll modify in conflicting ways:**

   ```bash
   cat > team-info.txt << 'EOF'
   Team: Development Team Alpha
   Project: Customer Management System
   Lead: TBD
   Technology: JavaScript
   Status: In Development
   Priority: High

   Team Members:
   - Alice (Backend Developer)
   - Bob (Frontend Developer)
   - Charlie (DevOps Engineer)
   EOF
   ```

3. **Make the initial commit:**

   ```bash
   git add team-info.txt
   git commit -m "Initial team information"
   ```

### Step 2: Create Conflicting Changes (8 minutes)

1. **Create and work on feature branch:**

   ```bash
   git checkout -b feature/update-team-lead
   ```

2. **Modify the team lead and add more details:**

   ```bash
   # Replace TBD with actual lead name and update status
   cat > team-info.txt << 'EOF'
   Team: Development Team Alpha
   Project: Customer Management System
   Lead: Sarah Johnson
   Technology: JavaScript, Node.js, React
   Status: Active Development
   Priority: High

   Team Members:
   - Alice (Backend Developer) - Node.js specialist
   - Bob (Frontend Developer) - React expert
   - Charlie (DevOps Engineer) - AWS certified
   - David (QA Engineer) - Automation specialist
   EOF
   ```

3. **Commit the feature branch changes:**

   ```bash
   git add team-info.txt
   git commit -m "feat: update team lead and expand member details"
   ```

4. **Switch back to main and make different changes:**

   ```bash
   git checkout main
   ```

5. **Make conflicting changes on main:**

   ```bash
   cat > team-info.txt << 'EOF'
   Team: Development Team Alpha
   Project: Customer Management System
   Lead: Michael Chen
   Technology: JavaScript, TypeScript
   Status: Planning Phase
   Priority: High

   Team Members:
   - Alice (Senior Backend Developer)
   - Bob (Senior Frontend Developer)
   - Charlie (DevOps Engineer)
   - Eve (UI/UX Designer)
   EOF
   ```

6. **Commit the main branch changes:**

   ```bash
   git add team-info.txt
   git commit -m "update: assign Michael as team lead and promote seniors"
   ```

### Step 3: Trigger the Merge Conflict (5 minutes)

1. **Attempt to merge the feature branch:**

   ```bash
   git merge feature/update-team-lead
   ```

   **Expected Result:** Git will report a merge conflict!

2. **Check the conflict status:**

   ```bash
   git status
   ```

3. **Examine the conflicted file:**

   ```bash
   cat team-info.txt
   ```

   **Note:** You'll see Git's conflict markers:

   - `<<<<<<< HEAD` - Start of your current branch (main)
   - `=======` - Separator between conflicting versions
   - `>>>>>>> feature/update-team-lead` - End of the branch being merged

### Step 4: Manual Conflict Resolution (8 minutes)

1. **Understand the conflict:**

   Look at the conflicted content and identify:

   - What changed on each branch?
   - Which changes should be kept?
   - How can we combine the best of both?

2. **Edit the file to resolve conflicts:**

   Open `team-info.txt` in your text editor and create a merged version:

   ```bash
   cat > team-info.txt << 'EOF'
   Team: Development Team Alpha
   Project: Customer Management System
   Lead: Sarah Johnson
   Technology: JavaScript, TypeScript, Node.js, React
   Status: Active Development
   Priority: High

   Team Members:
   - Alice (Senior Backend Developer) - Node.js specialist
   - Bob (Senior Frontend Developer) - React expert
   - Charlie (DevOps Engineer) - AWS certified
   - David (QA Engineer) - Automation specialist
   - Eve (UI/UX Designer)
   EOF
   ```

3. **Mark the conflict as resolved:**

   ```bash
   git add team-info.txt
   ```

4. **Complete the merge:**

   ```bash
   git commit -m "Merge feature/update-team-lead: combine team updates"
   ```

5. **Verify the merge:**

   ```bash
   git log --oneline --graph
   ```

### Step 5: Using Git Tools for Conflict Resolution (4 minutes)

Let's practice with Git's built-in merge tools:

1. **Create another conflict scenario:**

   ```bash
   git checkout -b feature/add-timeline
   echo "" >> team-info.txt
   echo "Project Timeline:" >> team-info.txt
   echo "- Phase 1: Requirements (2 weeks)" >> team-info.txt
   echo "- Phase 2: Development (8 weeks)" >> team-info.txt
   git add team-info.txt
   git commit -m "Add project timeline"

   git checkout main
   echo "" >> team-info.txt
   echo "Contact Information:" >> team-info.txt
   echo "- Slack: #team-alpha" >> team-info.txt
   echo "- Email: team-alpha@company.com" >> team-info.txt
   git add team-info.txt
   git commit -m "Add contact information"
   ```

2. **Trigger conflict and use mergetool:**

   ```bash
   git merge feature/add-timeline

   # If you have a merge tool configured (like VSCode, vimdiff, etc.)
   git mergetool

   # Or resolve manually as before
   ```

## Challenge Tasks (Additional Practice)

### Challenge 1: Binary File Conflicts

1. Create two branches that modify the same image file
2. Try to merge and see how Git handles binary conflicts
3. Practice choosing the correct version

### Challenge 2: Directory vs File Conflicts

1. Create a branch that creates a file named `docs`
2. Create another branch that creates a directory named `docs`
3. Merge and resolve the naming conflict

### Challenge 3: Complex Multi-File Conflicts

1. Create conflicts across multiple files simultaneously
2. Practice resolving them systematically
3. Use `git status` to track your progress

## Common Conflict Patterns & Solutions

### Pattern 1: Same Line, Different Content

**Scenario:** Two people edit the same line differently.

**Strategy:**

- Read both versions carefully
- Decide if you need one version, the other, or a combination
- Test the result if it's code

### Pattern 2: Nearby Line Changes

**Scenario:** Changes on adjacent lines that Git can't auto-merge.

**Strategy:**

- Usually both changes can coexist
- Combine them intelligently
- Maintain proper formatting/syntax

### Pattern 3: Addition vs Deletion

**Scenario:** One branch adds content where another deleted it.

**Strategy:**

- Understand why each change was made
- Decide if the addition is still relevant
- Consider the context of the deletion

## Conflict Resolution Best Practices

### Before Merging

1. **Pull latest changes** from main branch
2. **Test your branch** thoroughly
3. **Communicate** with team about potential conflicts

### During Resolution

1. **Don't rush** - understand what each side changed
2. **Test after resolving** - make sure code still works
3. **Ask for help** if you're unsure about someone else's changes
4. **Keep it simple** - don't add new features while resolving conflicts

### After Resolution

1. **Test thoroughly** - conflicts can introduce bugs
2. **Review the merge commit** before pushing
3. **Document** complex resolution decisions

## Verification & Reflection

### Verify Your Work

1. **Check that all conflicts are resolved:**

   ```bash
   git status
   ```

2. **Review your merge commits:**

   ```bash
   git log --oneline --graph -n 10
   ```

3. **Verify file contents make sense:**

   ```bash
   cat team-info.txt
   ```

### Reflection Questions

1. **What strategies helped you understand the conflicts better?**

2. **How would you communicate with teammates about conflict resolution?**

3. **What tools or IDE features might help with conflict resolution?**

4. **How can teams minimize conflicts in the first place?**

## Prevention Strategies

### Communication

- **Coordinate** who works on what files
- **Use feature branches** to isolate changes
- **Pull frequently** to stay up to date
- **Break down** large features into smaller pieces

### Technical Approaches

- **Frequent merging** from main to feature branches
- **Atomic commits** that change one thing at a time
- **Good branch naming** to indicate intent
- **Code reviews** to catch potential conflicts early

## Key Takeaways

- **Conflicts are normal** - they're part of collaborative development
- **Read carefully** - understand what each side changed and why
- **Test after resolving** - conflicts can introduce subtle bugs
- **Don't panic** - conflicts are fixable with patience and communication
- **Practice makes perfect** - the more you resolve conflicts, the easier it becomes

## Next Steps

This exercise prepares you for the intermediate commands session where you'll learn about stashing, rebasing, and other advanced conflict prevention techniques.
