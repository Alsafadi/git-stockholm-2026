# Instructor Cheat Sheet: Live Demo Commands (PowerShell)

Copy/paste-ready PowerShell for every "Demo" section in the Day 1 / Day 2 slide
decks. The slides themselves show `bash` for portability, but these are the
Windows/PowerShell equivalents to actually type (or paste) during class.

## One-time setup (before class)

```powershell
# Scratch folder for all live demos - keeps your real repos untouched
mkdir $HOME\git-course-demos -Force
cd $HOME\git-course-demos
```

To wipe everything and start a section fresh (safe to re-run any time):

```powershell
Set-Location $HOME\git-course-demos
Get-ChildItem | Remove-Item -Recurse -Force
```

---

## Day 1

### `day1/01-git-installation.md` — Demo Time!

_Steps: check install → identity → essential settings → verify → first repo_

```powershell
# 1. Check Git installation
git --version

# 2. Set up user identity
git config --global user.name "Your Full Name"
git config --global user.email "your.email@example.com"

# 3. Configure essential settings
git config --global init.defaultBranch main
git config --global core.autocrlf true
git config --global core.editor "code --wait"

# 4. Verify configuration
git config --list --show-origin

# 5. Test with first repository
mkdir demo-first-repo
cd demo-first-repo
git init
git status
cd ..
```

---

### `day1/02-gitignore-gitattributes.md` — Demo Time!

_Steps: test repo → various file types → .gitignore → test patterns → .gitattributes → effects_

```powershell
# 1. Create a test repository
mkdir gitignore-demo
cd gitignore-demo
git init

# 2. Add various file types
Set-Content -Encoding utf8 app.js "console.log('hi');"
Set-Content -Encoding utf8 debug.log "debug output"
mkdir node_modules
Set-Content -Encoding utf8 node_modules\fake-lib.js "fake dependency"
git status

# 3. Create .gitignore
@'
*.log
node_modules/
'@ | Set-Content -Encoding utf8 .gitignore

# 4. Test ignore patterns
git status
git status --ignored
git check-ignore -v debug.log

# 5. Add .gitattributes
@'
* text=auto
*.sh text eol=lf
*.bat text eol=crlf
'@ | Set-Content -Encoding utf8 .gitattributes

# 6. See the effects
git add .
git status
git check-attr text app.js

cd ..
```

---

### `day1/03-core-concepts-pt1.md` — Demo: Creating Your First Repository

```powershell
# Create new repository
mkdir my-project
cd my-project
git init

# Check status
git status

# Create a file
Set-Content -Encoding utf8 hello.txt "Hello Git!"

# See Git's perspective
git status
```

### `day1/03-core-concepts-pt1.md` — Demo: Making Your First Commit

_(continues in the same `my-project` folder)_

```powershell
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

cd ..
```

---

### `day1/05-basic-commands.md` — git init Demo

```powershell
# Create project directory
mkdir my-awesome-project
cd my-awesome-project

# Initialize Git repository
git init

# See what was created
Get-ChildItem -Force
# Look for: .git/ directory

# Check status
git status
# Output: "On branch main, No commits yet"
```

### `day1/05-basic-commands.md` — git add Demo

_(continues in the same `my-awesome-project` folder)_

```powershell
# Create some files
Set-Content -Encoding utf8 hello.txt "Hello World"
Set-Content -Encoding utf8 app.js "console.log('Hi')"
mkdir src
Set-Content -Encoding utf8 src\utils.js "export default {}"

# Check status
git status

# Stage one file
git add hello.txt

# Check status again
git status

# Stage remaining files
git add .

cd ..
```

---

### `day1/06-core-concepts-pt2.md` — Demo: Create and Connect Repository

**On GitHub (browser, not scripted):**

1. Create new repository
2. Copy the clone URL

**Locally — pick ONE of the two:**

```powershell
# Option 1: Clone first
git clone https://github.com/<user>/<repo>.git

# Option 2: Connect an existing local repo
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```

---

## Day 2

### `day2/01-core-concepts-pt3.md` — Branch Workflow Demo

_(run inside any existing demo repo with a `main` branch, e.g. `my-project`)_

```powershell
# Start on main
git switch main

# Create feature branch
git switch -c add-user-auth

# Make changes
Set-Content -Encoding utf8 auth.js "function login() {}"
git add auth.js
git commit -m "feat: add login function"

# More changes
Add-Content -Encoding utf8 auth.js "function logout() {}"
git add auth.js
git commit -m "feat: add logout function"

# Check history
git log --oneline
```

### `day2/01-core-concepts-pt3.md` — Demo: Complete Branch Workflow

```powershell
mkdir branch-workflow-demo
cd branch-workflow-demo
git init
Set-Content -Encoding utf8 README.md "# Branch Workflow Demo"
git add .
git commit -m "Initial commit"

# Create feature branch
git switch -c feature/add-navbar

# Make changes
Set-Content -Encoding utf8 navbar.html "<nav>Navigation</nav>"
git add navbar.html
git commit -m "feat: add navigation bar"

# Switch back and merge
git switch main
git merge feature/add-navbar

# Clean up
git branch -d feature/add-navbar

cd ..
```

_Note: the slide's original version starts with `git switch main; git pull origin main` — drop the `pull` for a local-only demo since there's no remote here._

---

### `day2/02-merging-conflicts.md` — Creating a Conflict (Demo Setup)

```powershell
# Create test repository
git init conflict-demo
cd conflict-demo

# Initial commit
Set-Content -Encoding utf8 file.txt "Original content"
git add file.txt
git commit -m "Initial commit"

# Create feature branch
git switch -c feature
Set-Content -Encoding utf8 file.txt "Feature content"
git add file.txt
git commit -m "Feature changes"

# Switch to main and make conflicting change
git switch main
Set-Content -Encoding utf8 file.txt "Main content"
git add file.txt
git commit -m "Main changes"

# Try to merge - CONFLICT!
git merge feature
```

### `day2/02-merging-conflicts.md` — Live Demo: Resolving Real Conflicts

_(continues directly from the conflict created above — `git status` will show `file.txt` as unmerged)_

```powershell
git status

# Open file.txt in your editor and resolve the <<<<<<< / ======= / >>>>>>> markers.
# For a scripted resolution to paste instead:
'Original content

Resolved: keeping both perspectives (main + feature)' | Set-Content -Encoding utf8 file.txt

git add file.txt
git commit -m "Merge feature: resolve content conflict"

git log --oneline --graph

cd ..
```

_The slide's other two scenarios (code-logic conflict, config/API-endpoint conflict) are intentionally open-ended — reuse this same `git switch -c ...` / edit-both-branches / `git merge` pattern with different file content if you want a second live example._

---

## Quick reference: bash → PowerShell used above

| Slide shows (`bash`)        | Use instead (PowerShell)                          |
| ---------------------------- | --------------------------------------------------- |
| `echo "text" > file`         | `Set-Content -Encoding utf8 file "text"`             |
| `echo "text" >> file`        | `Add-Content -Encoding utf8 file "text"`             |
| `cat > file << 'EOF' ... EOF`| `@'...'@ \| Set-Content -Encoding utf8 file`         |
| `cat file`                   | `Get-Content file`                                   |
| `ls -la`                     | `Get-ChildItem -Force`                               |
| `mkdir -p a/b/c`              | `mkdir a\b\c` (PowerShell creates parents natively) |
| `rm -rf dir`                  | `Remove-Item -Recurse -Force dir`                    |
| `grep pattern file`           | `Select-String pattern file`                         |
