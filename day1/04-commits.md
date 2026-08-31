# Commit Messages & Conventions

_Writing clear, meaningful commit messages_

---

## Why Commit Messages Matter

### Your commit history is:

- **Documentation** of what changed and why
- **Communication** tool for your team
- **Debugging** aid when things break
- **Release notes** foundation
- **Code review** context

### Bad messages = Lost context forever

---

## Anatomy of a Good Commit Message

```
[type](scope): Short description (50 chars max)

Longer explanation of what changed and why.
Wrap at 72 characters. This section is optional
but helpful for complex changes.

- Bullet points are okay
- Use present tense: "Add feature" not "Added feature"
- Reference issues: Fixes #123
```

---

## The Two-Part Structure

### Subject Line (Required):

- **50 characters max**
- **Imperative mood** ("Add" not "Added")
- **No period** at the end
- **Capitalize** first letter

### Body (Optional):

- **Separated by blank line**
- **72 characters per line**
- **Explain what and why**, not how
- **Can include** bullet points

---

## Subject Line Examples

### ✅ Good:

```
Add user authentication feature
Fix login button alignment issue
Update README with installation steps
Remove deprecated API endpoints
Refactor database connection logic
```

### ❌ Bad:

```
fixed stuff
WIP
asdf
Updated files
Bug fix
small change
```

---

## Conventional Commit Format

**Popular standard for commit messages:**

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Types:

- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation
- **style**: Formatting, no code change
- **refactor**: Code restructuring
- **test**: Adding tests
- **chore**: Maintenance

---

## Conventional Commit Examples

```bash
feat(auth): add login functionality

fix(ui): correct button alignment on mobile

docs(readme): update installation instructions

style(css): format stylesheets with prettier

refactor(api): simplify user data validation

test(auth): add unit tests for login service

chore(deps): update npm dependencies
```

---

## When to Include a Body

### Always include body for:

- **Complex changes** requiring explanation
- **Breaking changes**
- **Business logic** decisions
- **Performance** considerations
- **Security** implications

### Example:

```
refactor(auth): switch from JWT to session-based auth

JWT tokens were causing issues with mobile apps due to
storage limitations. Session-based authentication provides
better security and simpler implementation.

This is a breaking change that requires users to log in again.
```

---

## Commit Message Templates

Create a template to maintain consistency:

```bash
# Set global commit template
git config --global commit.template ~/.gitmessage

# Create template file
cat > ~/.gitmessage << EOF
# <type>[optional scope]: <description>
#
# [optional body]
#
# [optional footer(s)]

# Types:
# feat: A new feature
# fix: A bug fix
# docs: Documentation only changes
# style: Formatting, missing semi colons, etc
# refactor: Code change that neither fixes a bug nor adds a feature
# test: Adding missing tests
# chore: Changes to build process or auxiliary tools
EOF
```

---

## Common Commit Message Patterns

### Feature Development:

```
feat(user-profile): add avatar upload functionality
feat(search): implement full-text search
feat(api): add user preferences endpoint
```

### Bug Fixes:

```
fix(login): resolve session timeout issue
fix(ui): correct responsive layout on tablets
fix(validation): handle empty email addresses
```

### Documentation:

```
docs(api): add authentication examples
docs(readme): clarify installation requirements
docs(contributing): add code style guidelines
```

---

## Referencing Issues and PRs

### Link to issue trackers:

```
fix(auth): resolve login redirect loop

The authentication flow was causing infinite redirects
when users had expired sessions.

Fixes #123
Closes #456
Resolves #789
```

### Reference pull requests:

```
feat(dashboard): add user analytics widget

See PR #42 for detailed implementation discussion.
Related to #38, #41
```

---

## Breaking Changes

### Mark breaking changes clearly:

```
feat(api)!: remove deprecated user endpoints

BREAKING CHANGE: The /api/v1/users endpoint has been
removed. Use /api/v2/users instead.

Migration guide available at docs/migration.md
```

### Alternative format:

```
feat(api): remove deprecated user endpoints

BREAKING CHANGE: The /api/v1/users endpoint has been
removed. Use /api/v2/users instead.
```

---

## Team Conventions

### Establish team standards:

- **Agree on commit types** (feat, fix, docs, etc.)
- **Define scope naming** (component names, modules)
- **Set length limits** (50/72 character rules)
- **Reference format** (issue numbers, PR links)
- **Review process** (commit message quality checks)

### Tools to enforce:

- **commitlint** - Lint commit messages
- **husky** - Git hooks for validation
- **commitizen** - Interactive commit wizard

---

## Tools for Better Commit Messages

### Commitizen:

```bash
npm install -g commitizen
npm install -g cz-conventional-changelog

# Use instead of git commit
git cz
```

### commitlint:

```bash
npm install --save-dev @commitlint/{cli,config-conventional}

# Add to package.json
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

---

## Editing Commit Messages

### Change last commit message:

```bash
git commit --amend -m "New commit message"
```

### Interactive editor:

```bash
git commit --amend
```

### Change older commits:

```bash
git rebase -i HEAD~3  # Edit last 3 commits
```

---

## Commit Message Anti-Patterns

### ❌ Avoid these:

```
Fix
Update
WIP
temp
asdf
Fixed bug
Updated code
Minor changes
Stuff
Quick fix
```

### ✅ Instead write:

```
fix(auth): resolve session expiration bug
feat(ui): update navigation design
refactor(db): optimize query performance
docs(api): clarify rate limiting behavior
```

---

## Real-World Examples

### From popular open source projects:

**React:**

```
feat(react-dom): add support for CSS custom properties
fix(react): prevent infinite loop in useEffect
```

**Vue.js:**

```
feat(compiler-sfc): support defineModel macro
fix(reactivity): ensure computed values are cached
```

**VS Code:**

```
feat(editor): add sticky scroll for nested scopes
fix(terminal): resolve PATH issues on macOS
```

---

## Writing Commit Messages Exercise

### Practice with these scenarios:

1. **You added** a new search feature to your app
2. **You fixed** a bug where users couldn't log out
3. **You updated** the README file with new examples
4. **You refactored** the database connection code
5. **You removed** an old, unused component

**Write good commit messages for each!**

---

## Commit Message Checklist

### Before committing, ask:

- ✅ **Does the subject line** describe what changed?
- ✅ **Is it under** 50 characters?
- ✅ **Uses imperative** mood? ("Add" not "Added")
- ✅ **Is the body** needed for context?
- ✅ **References** relevant issues/PRs?
- ✅ **Follows team** conventions?

---

## The Impact of Good Messages

### Benefits:

- **Faster debugging** - find when bugs were introduced
- **Better code reviews** - understand the context
- **Easier releases** - generate changelogs automatically
- **Team communication** - know what everyone is working on
- **Future maintenance** - understand historical decisions

### **Your future self will thank you!**

---

## Questions About Commit Messages?

**Good commit messages are a skill that pays dividends forever**

**Next:** Basic Git Commands (after lunch break)
