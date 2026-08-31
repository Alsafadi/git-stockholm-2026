# Tagging: Annotated & Lightweight Tags

_Marking important points in your project history_

---

### What are Git Tags?

#### Definition:

**Tags are pointers to specific commits that don't move**

#### Unlike branches:

- **Branches** move as you add commits
- **Tags** stay fixed at specific commits
- **Perfect** for marking releases, versions, milestones

#### Common uses:

- **Version releases** (v1.0.0, v2.1.3)
- **Stable snapshots**
- **Important milestones**
- **Deployment markers**

---

### Two Types of Tags

#### 1. Lightweight Tags

- **Simple pointer** to a commit
- **Just a name** (like a branch that doesn't move)
- **No additional** metadata
- **Quick** and minimal

#### 2. Annotated Tags

- **Full objects** in Git database
- **Contains metadata** (tagger, date, message)
- **Can be signed** with GPG
- **Recommended** for releases

---

### Creating Lightweight Tags

#### Basic syntax:

```bash
# Tag current commit
git tag v1.0.0

# Tag specific commit
git tag v1.0.0 abc123

# List all tags
git tag

# Show tag details
git show v1.0.0
```

#### Example:

```bash
# Mark current state as version 1.0
git tag v1.0.0

# Verify
git tag
# Output: v1.0.0
```

---

### Creating Annotated Tags

#### Basic syntax:

```bash
# Create annotated tag with message
git tag -a v1.0.0 -m "Release version 1.0.0"

# Create annotated tag with editor
git tag -a v1.0.0

# Tag specific commit
git tag -a v1.0.0 abc123 -m "Version 1.0.0"
```

#### Example:

```bash
git tag -a v1.2.0 -m "Version 1.2.0

Features:
- User authentication
- Password reset
- Email notifications

Bug fixes:
- Login redirect issue
- Session timeout problem"
```

---

### Semantic Versioning

#### Format: MAJOR.MINOR.PATCH

- **MAJOR** - Breaking changes (v1.0.0 → v2.0.0)
- **MINOR** - New features, backward compatible (v1.1.0 → v1.2.0)
- **PATCH** - Bug fixes, backward compatible (v1.1.1 → v1.1.2)

#### Examples:

```bash
git tag -a v1.0.0 -m "Initial release"
git tag -a v1.1.0 -m "Add user profiles"
git tag -a v1.1.1 -m "Fix login bug"
git tag -a v2.0.0 -m "New API - breaking changes"
```

---

### Viewing Tags

#### List tags:

```bash
# All tags
git tag

# Pattern matching
git tag -l "v1.*"
git tag --list "v2.1.*"

# Sort by version
git tag --sort=version:refname
```

#### Show tag information:

```bash
# Lightweight tag
git show v1.0.0

# Annotated tag (shows metadata)
git show v1.2.0
```

---

### Tag Metadata Example

#### Annotated tag output:

```
tag v1.2.0
Tagger: Jane Doe <jane@example.com>
Date:   Mon Oct 13 14:30:22 2025 +0100

Version 1.2.0

Features:
- User authentication
- Password reset

commit abc123def456...
Author: Jane Doe <jane@example.com>
Date:   Mon Oct 13 14:25:15 2025 +0100

    feat: add user authentication
```

---

### Working with Remote Tags

#### Push tags:

```bash
# Push single tag
git push origin v1.0.0

# Push all tags
git push origin --tags

# Push all (commits + tags)
git push --follow-tags
```

#### Fetch tags:

```bash
# Fetch all tags
git fetch --tags

# Fetch specific tag
git fetch origin tag v1.0.0
```

---

### Checking Out Tags

#### View tagged state:

```bash
# Checkout tag (detached HEAD)
git checkout v1.0.0

# Create branch from tag
git checkout -b hotfix-v1.0.1 v1.0.0
```

#### Detached HEAD warning:

```
Note: switching to 'v1.0.0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.
```

---

### Deleting Tags

#### Local tags:

```bash
# Delete local tag
git tag -d v1.0.0

# Delete multiple tags
git tag -d v1.0.0 v1.0.1
```

#### Remote tags:

```bash
# Delete remote tag
git push origin :refs/tags/v1.0.0
# or newer syntax
git push origin --delete v1.0.0
```

---

### Tag Naming Conventions

#### Version tags:

```bash
v1.0.0        # Semantic versioning
1.0.0         # Without 'v' prefix
v1.0.0-beta   # Pre-release
v1.0.0-rc1    # Release candidate
```

#### Other tags:

```bash
release-2025-10-13
milestone-alpha
stable-build
production-deploy
```

#### Choose convention and stick to it!

---

### Practical Tagging Workflow

#### 1. Prepare release:

```bash
# Ensure clean state
git status

# Update version in files
# Update CHANGELOG
git add .
git commit -m "chore: prepare v1.2.0 release"
```

#### 2. Create tag:

```bash
git tag -a v1.2.0 -m "Release v1.2.0

New features:
- User dashboard
- Export functionality

Bug fixes:
- Memory leak in processor
- UI alignment issues"
```

#### 3. Push release:

```bash
git push origin main
git push origin v1.2.0
```

---

### Release Workflow with Tags

#### Complete example:

```bash
# 1. Finish feature development
git switch main
git pull origin main
git merge feature/user-dashboard

# 2. Update version files
echo "1.2.0" > VERSION
git add VERSION
git commit -m "chore: bump version to 1.2.0"

# 3. Create release tag
git tag -a v1.2.0 -m "Release version 1.2.0"

# 4. Push everything
git push origin main --follow-tags

# 5. Create GitHub release (optional)
# Use GitHub web interface or CLI
```

---

### Finding Tags

#### Search tags:

```bash
# Find tags containing commit
git tag --contains abc123

# Find commits between tags
git log v1.0.0..v1.1.0

# Show tag dates
git for-each-ref --format='%(refname:short) %(taggerdate)' refs/tags
```

#### Compare versions:

```bash
# See changes between versions
git diff v1.0.0..v1.1.0

# Show log between versions
git log --oneline v1.0.0..v1.1.0
```

---

### Signed Tags

#### Create signed tag:

```bash
# Requires GPG key setup
git tag -s v1.0.0 -m "Signed release v1.0.0"
```

#### Verify signed tag:

```bash
git tag -v v1.0.0
```

#### Benefits:

- **Authenticity** verification
- **Tamper** detection
- **Trust** in releases

---

### Integration with GitHub/GitLab

#### GitHub Releases:

- **Auto-created** from tags
- **Release notes** from tag message
- **Asset uploads** (binaries, docs)
- **Pre-release** marking

#### GitLab Releases:

- **Similar** to GitHub
- **CI/CD** integration
- **Container** registry tags
- **Deployment** automation

---

### Best Practices

#### Tagging strategy:

✅ **Use semantic** versioning  
✅ **Annotated tags** for releases  
✅ **Descriptive** messages  
✅ **Consistent** naming  
✅ **Tag stable** points

#### Avoid:

❌ **Moving/changing** tags  
❌ **Tagging** unstable code  
❌ **Inconsistent** naming  
❌ **Empty** tag messages

---

### Common Use Cases

#### Version releases:

```bash
git tag -a v2.1.0 -m "Version 2.1.0 - Performance improvements"
```

#### Hotfixes:

```bash
git tag -a v2.0.1 -m "Hotfix v2.0.1 - Security patch"
```

#### Milestones:

```bash
git tag -a milestone-beta -m "Beta milestone reached"
```

#### Deployments:

```bash
git tag -a deploy-prod-20251013 -m "Production deployment Oct 13"
```

---

### Troubleshooting Tags

#### Tag already exists:

```bash
# Force update (dangerous)
git tag -f v1.0.0

# Delete and recreate
git tag -d v1.0.0
git tag -a v1.0.0 -m "Updated release"
```

#### Wrong commit tagged:

```bash
# Delete and recreate at correct commit
git tag -d v1.0.0
git tag -a v1.0.0 abc123 -m "Release v1.0.0"
```

---

### Questions About Tagging?

**Tags are essential for release management**

**Coming up:** Intermediate Git commands

**Any questions about creating or managing tags?**
