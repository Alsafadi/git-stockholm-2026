# Merge Conflicts: Live Resolution Demo

_Understanding and resolving merge conflicts like a pro_

---

### What is a Merge Conflict?

#### Definition:

**A situation where Git cannot automatically merge changes because the same part of a file was modified differently in two branches**

#### When conflicts happen:

- **Same lines** changed differently
- **One branch** deletes file, another modifies it
- **Binary files** changed in both branches
- **File/directory** naming conflicts

---

### How Conflicts Occur

#### Scenario:

```
main:     A --- B --- C (edit line 5: "Hello World")
               \
feature:        D --- E (edit line 5: "Hello Git")
```

#### When merging:

Git doesn't know which version to keep:

- "Hello World" (from main)
- "Hello Git" (from feature)

**Human decision required!**

---

### Conflict Markers

#### Git inserts special markers:

```javascript
function greet() {
<<<<<<< HEAD
    return "Hello World!";
=======
    return "Hello Git!";
>>>>>>> feature-branch
}
```

#### Explanation:

- `<<<<<<< HEAD` - Start of current branch version
- `=======` - Separator between versions
- `>>>>>>> feature-branch` - End of incoming branch version

---

### Types of Conflicts

#### 1. Content Conflicts

Same lines modified differently

```
<<<<<<< HEAD
const API_URL = "https://api.example.com";
=======
const API_URL = "https://dev-api.example.com";
>>>>>>> feature
```

#### 2. Add/Add Conflicts

Same file added with different content

#### 3. Modify/Delete Conflicts

One branch modifies, another deletes

#### 4. Rename Conflicts

File renamed differently in both branches

---

### Creating a Conflict (Demo Setup)

Let's create a conflict intentionally:

```bash
# Create test repository
git init conflict-demo
cd conflict-demo

# Initial commit
echo "Original content" > file.txt
git add file.txt
git commit -m "Initial commit"

# Create feature branch
git switch -c feature
echo "Feature content" > file.txt
git add file.txt
git commit -m "Feature changes"

# Switch to main and make conflicting change
git switch main
echo "Main content" > file.txt
git add file.txt
git commit -m "Main changes"

# Try to merge - CONFLICT!
git merge feature
```

---

### Identifying Conflicts

#### Git status during conflict:

```bash
$ git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   file.txt
```

#### Key indicators:

- **"You have unmerged paths"**
- **"both modified"** status
- **No clean working directory**

---

### Resolving Conflicts Manually

#### Step 1: Open conflicted file

```javascript
<<<<<<< HEAD
function calculateTotal(price, tax) {
    return price + (price * tax);
}
=======
function calculateTotal(price, taxRate) {
    const total = price + (price * taxRate);
    return Math.round(total * 100) / 100;
}
>>>>>>> feature-enhanced-calc
```

#### Step 2: Choose resolution

- Keep HEAD version
- Keep feature version
- Combine both
- Write completely new version

---

### Resolution Strategies

#### Option 1: Keep current branch (HEAD)

```javascript
function calculateTotal(price, tax) {
  return price + price * tax;
}
```

#### Option 2: Keep incoming branch

```javascript
function calculateTotal(price, taxRate) {
  const total = price + price * taxRate;
  return Math.round(total * 100) / 100;
}
```

#### Option 3: Combine both (most common)

```javascript
function calculateTotal(price, taxRate) {
  return price + price * taxRate;
}
```

---

### Resolution Process

#### 1. Edit the file manually:

```bash
# Remove conflict markers
# Choose desired content
nano file.txt  # or use your editor
```

#### 2. Stage the resolved file:

```bash
git add file.txt
```

#### 3. Complete the merge:

```bash
git commit -m "Merge feature-enhanced-calc

Resolved conflicts in calculateTotal function.
Combined parameter naming from feature branch
with simpler logic from main branch."
```

---

### Using Merge Tools

#### Configure merge tool:

```bash
# VS Code
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Vim
git config --global merge.tool vimdiff

# Beyond Compare
git config --global merge.tool bc3
```

#### Use merge tool:

```bash
git mergetool
```

**Opens visual interface for conflict resolution**

---

### GUI Conflict Resolution

#### VS Code:

- **Built-in** Git support
- **Side-by-side** comparison
- **Click** to accept changes
- **IntelliSense** for code conflicts

#### GitKraken:

- **Visual** conflict editor
- **Chunk-by-chunk** resolution
- **Syntax highlighting**

#### SourceTree:

- **Integrated** merge tool
- **External tool** integration

---

### Command Line Conflict Resolution

#### View conflict files:

```bash
git diff --name-only --diff-filter=U
```

#### Show conflict details:

```bash
git diff
```

#### Check merge status:

```bash
git status
```

#### Abort merge if needed:

```bash
git merge --abort
```

---

### Live Demo: Resolving Real Conflicts

Let's resolve conflicts in different scenarios:

#### Scenario 1: Simple text conflict

- **Two developers** edit same line
- **Different** welcome messages
- **Choose** the better version

#### Scenario 2: Code logic conflict

- **Different** algorithms
- **Same** function name
- **Combine** the best parts

#### Scenario 3: Configuration conflict

- **Different** API endpoints
- **Environment-specific** settings
- **Keep** both with conditionals

---

### Best Practices for Conflict Resolution

#### Before resolving:

✅ **Understand** both changes  
✅ **Communicate** with other developers  
✅ **Test** the resolution  
✅ **Consider** the bigger picture

#### During resolution:

✅ **Remove all** conflict markers  
✅ **Ensure** code still works  
✅ **Follow** coding standards  
✅ **Write** descriptive commit message

---

### Preventing Conflicts

#### Team strategies:

- **Small, frequent** commits
- **Short-lived** branches
- **Regular** merging from main
- **Communication** about overlapping work
- **Code ownership** agreements

#### Technical strategies:

- **Modular** code structure
- **Clear** separation of concerns
- **Configuration** files over hardcoded values
- **Automatic** code formatting

---

### Complex Conflict Scenarios

#### Multiple files with conflicts:

```bash
git status
# Shows all conflicted files
# Resolve each one individually
git add file1.js file2.js file3.js
git commit
```

#### Binary file conflicts:

```bash
# Choose one version
git checkout --theirs binary-file.png
# or
git checkout --ours binary-file.png
git add binary-file.png
```

---

### Conflict Resolution Tools Comparison

#### Manual editing:

- Full control
- Works anywhere
- Can be tedious

#### Git mergetool:

- Visual interface
- Side-by-side comparison
- Needs configuration

#### IDE integration (My go to):

- Familiar environment
- Syntax highlighting
- Integrated testing

---

### After Conflict Resolution

#### Verify the merge:

```bash
# Check that it compiles/runs
npm run dev  # or your test command

# Review the merge commit
git show HEAD

# Check the history
git log --oneline --graph -10
```

#### Clean up:

```bash
# Delete merged branch
git branch -d feature-branch

# Push the merge
git push origin main
```

---

### Troubleshooting Conflicts

#### "I messed up the resolution"

```bash
# Reset to before merge
git reset --hard HEAD~1

# Or abort and start over
git merge --abort
git merge feature-branch
```

#### "Too many conflicts"

```bash
# Consider rebasing instead
git merge --abort
git rebase main feature-branch
```

#### "Can't understand the conflict"

```bash
# Get help from team
# Use visual tools
# Check git log for context
```

---

### Merge Strategies

#### Recursive (default):

- **Good** for most situations
- **Handles** renames well
- **Three-way** merge

#### Octopus:

- **Multiple** branches at once
- **Only** if no conflicts
- **Rarely** used manually

#### Ours/Theirs:

```bash
git merge -X ours feature    # Prefer our changes
git merge -X theirs feature  # Prefer their changes
```

---

### Questions About Conflicts?

**Conflict resolution is a critical Git skill**

**Coming up:** Tagging for releases

**Any questions about handling merge conflicts?**
