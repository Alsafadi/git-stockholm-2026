# Core Concepts 2: Local vs Remote, Hosting Platforms

_Understanding Git's distributed nature and cloud hosting_

---

### Local vs Remote: The Big Picture

#### Local Repository:

- **On your machine**
- **Complete history**
- **Work offline**
- **Fast operations**

#### Remote Repository:

- **On server/cloud**
- **Shared with team**
- **Backup/collaboration**
- **Synchronization point**

**Key insight: Both are complete, independent repositories**

---

<!-- .slide: class="somecode" -->

### The Distributed Model

```
    Alice's Computer          Server/Cloud         Bob's Computer
  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
  │  Local Repo     │    │  Remote Repo    │    │  Local Repo     │
  │  (complete)     │◄──►│  (complete)     │◄──►│  (complete)     │
  │  All commits    │    │  All commits    │    │  All commits    │
  │  All branches   │    │  All branches   │    │  All branches   │
  │  All history    │    │  All history    │    │  All history    │
  └─────────────────┘    └─────────────────┘    └─────────────────┘
```

**No single source of truth - every copy is equal**

---

### Why Distributed?

#### Advantages:

- **Work offline** - No network needed for commits, branches, history
- **Fast operations** - Everything is local
- **Multiple backups** - Every clone is a full backup
- **Flexible workflows** - Many ways to collaborate
- **No single point of failure** - Server down? Keep working

#### Traditional centralized VCS:

- **Always need network** connection
- **Server failure** = work stops
- **Limited offline** capabilities

---

### What is a Remote?

A **remote** is a reference to another repository

#### Common scenarios:

- **origin** - The repository you cloned from
- **upstream** - The original repository you forked
- **production** - Repository for deployment
- **backup** - Additional backup location

#### Remotes are just:

- **Name** (like "origin")
- **URL** (where the repository lives)
- **Reference** (not a copy)

---

### Remote Operations

#### Push:

**Send your commits** to remote repository

```bash
git push origin main
```

#### Pull:

**Get commits** from remote repository

```bash
git pull origin main
```

#### Fetch:

**Download commits** without merging

```bash
git fetch origin
```

---

### Git Hosting Platforms

**Where to store your remote repositories?**

---

### GitHub

#### Overview:

- **Largest** Git hosting platform
- **Microsoft-owned** (since 2018)
- **100M+ repositories**
- **Free** for public repositories

#### Features:

- **Pull requests** for code review
- **Issues** for bug tracking
- **Actions** for CI/CD
- **Pages** for static websites
- **Codespaces** for cloud development

---

### GitHub Plans

#### Free:

- **Unlimited** public repositories
- **Unlimited** private repositories
- **Unlimited** collaborators
- **2000 Actions minutes/month**
- **500MB package storage**

#### Team ($4/user/month):

- **3000 Actions minutes/month**
- **2GB package storage**
- **Advanced** code review tools (required reviewers, protected branches)

---

### GitLab

#### Overview:

- **Complete DevOps** platform
- **Self-hosted** or cloud
- **Strong CI/CD** integration
- **Enterprise focus**

#### Features:

- **Built-in CI/CD** (very powerful)
- **Issue tracking** with boards
- **Container registry**
- **Security scanning**
- **Project management** tools

#### Unique advantages:

- **More CI/CD minutes** on free plan
- **Self-hosting** option
- **Integrated** DevOps tools

---

### Bitbucket

#### Overview:

- **Atlassian product**
- **Integrates** with Jira, Confluence
- **Enterprise focused**
- **Smaller** than GitHub/GitLab

#### Features:

- **Pipelines** for CI/CD
- **Built-in** Jira integration
- **IP whitelisting**
- **Branch permissions**

#### Best for:

- **Atlassian ecosystem** users
- **Enterprise** environments
- **Jira/Confluence** integration

---

### Other Hosting Options

#### Azure DevOps:

- **Microsoft's** platform
- **Integrates** with Azure services
- **Good for** .NET development

#### SourceForge:

- **Older platform**
- **Still active** for open source
- **Free hosting**

#### Self-hosted:

- **GitLab CE** (Community Edition)
- **Gitea** (lightweight)
- **Gogs** (simple)

---

### Choosing a Platform

#### Consider:

- **Team size** and collaboration needs
- **CI/CD** requirements
- **Integration** with other tools
- **Budget** constraints
- **Security** requirements
- **Storage** needs

#### For beginners:

**GitHub** - largest community, most resources  
**GitLab** - if you need powerful CI/CD  
**Bitbucket** - if using Atlassian tools

---

### Setting Up Remote Repository

#### Create on GitHub:

1. **Sign up** at github.com
2. **Click** "New repository"
3. **Choose** name and settings
4. **Copy** repository URL

#### Connect local to remote:

```bash
# If starting with existing local repo
git remote add origin https://github.com/user/repo.git
git push -u origin main

# If starting with remote repo
git clone https://github.com/user/repo.git
```

---

### Working with Remotes

#### View remotes:

```bash
git remote -v
# origin  https://github.com/user/repo.git (fetch)
# origin  https://github.com/user/repo.git (push)
```

#### Add remote:

```bash
git remote add origin https://github.com/user/repo.git
```

#### Change remote URL:

```bash
git remote set-url origin https://github.com/user/new-repo.git
```

#### Remove remote:

```bash
git remote remove origin
```

---

### SSH vs HTTPS

#### HTTPS:

```bash
https://github.com/user/repo.git
```

- **Username/password** required
- **Works everywhere**
- **Easier setup**
- **Personal access tokens** for security

#### SSH:

```bash
git@github.com:user/repo.git
```

- **SSH key** authentication
- **No password prompts**
- **More secure**
- **Faster** for frequent operations

---

### Setting Up SSH Keys

#### 1. Generate SSH key:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

#### 2. Add to SSH agent:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

#### 3. Copy public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

#### 4. Add to your hosting platform:

- **GitHub:** Settings → SSH and GPG keys → New SSH key
- **GitLab:** Preferences → SSH Keys → Add new key
- **Bitbucket:** Personal Bitbucket settings → Security → SSH keys → Add key

---

### Collaboration Workflows

#### Centralized Workflow:

```
Developer A  →  Central Repo  ←  Developer B
                       push             pull
```

#### Feature Branch Workflow:

```
main branch:     A --- B --- C --- D
                                   \           /
feature branch:         E --- F ---
```

#### Fork and Pull Request:

```
Original Repo  ←  Pull Request  ←  Forked Repo
     main -----------------------   feature
```

_Same idea on GitHub, GitLab, and Bitbucket - GitLab just calls it a **merge request** instead of a pull request_

---

### Repository Visibility

#### Public Repositories:

- **Anyone** can see and clone
- **Good for** open source projects
- **Search engines** can index
- **Free** on most platforms

#### Private Repositories:

- **Only invited** users can access
- **Good for** proprietary code
- **Team collaboration**
- **May cost money** depending on platform

#### Internal (Enterprise):

- **Organization members** only
- **Between** public and private
- **Enterprise** feature

---

### Demo: Create and Connect Repository

Let's create a repository and connect it:

#### On GitHub:

1. **Create new repository**
2. **Copy clone URL**

#### Locally:

```bash
# Option 1: Clone first
git clone https://github.com/user/repo.git

# Option 2: Connect existing
git remote add origin https://github.com/user/repo.git
git push -u origin main
```

---

### Understanding git push

```bash
# Basic push
git push origin main

# Set upstream (first time)
git push -u origin main

# Push all branches
git push --all origin

# Force push (dangerous!)
git push --force origin main
```

#### What happens:

1. **Local commits** sent to remote
2. **Remote branch** updated
3. **Tracking relationship** established (with -u)

---

### Understanding git pull

```bash
# Basic pull
git pull origin main

# Pull with rebase
git pull --rebase origin main

# Pull all branches
git pull --all
```

#### What happens:

1. **Fetch** commits from remote
2. **Merge** into current branch
3. **Update** working directory

#### git pull = git fetch + git merge

---

### Remote Branch Tracking

#### Tracking branches:

```bash
# See tracking relationships
git branch -vv

# Set up tracking
git branch --set-upstream-to=origin/main main

# Push and set tracking
git push -u origin feature-branch
```

#### Benefits:

- **git push/pull** without specifying remote
- **Status information** about ahead/behind
- **Simplified commands**

---

### Synchronization Strategies

#### Always pull before push:

```bash
git pull origin main
# resolve any conflicts
git push origin main
```

#### Use fetch to check first:

```bash
git fetch origin
git log HEAD..origin/main  # see what's new
git merge origin/main      # or git pull
```

#### Rebase to keep clean history:

```bash
git pull --rebase origin main
```

---

### Common Remote Scenarios

#### Fork and contribute:

1. **Fork** repository on GitHub
2. **Clone** your fork
3. **Create** feature branch
4. **Make** changes and commit
5. **Push** to your fork
6. **Create** pull request

#### Collaborate on team repo:

1. **Clone** shared repository
2. **Create** feature branch
3. **Push** feature branch
4. **Create** pull request
5. **Review** and merge

---

### Platform-Specific Features

#### GitHub:

- **Actions** for CI/CD
- **Pages** for hosting
- **Codespaces** for development
- **Discussions** for community

#### GitLab:

- **Built-in CI/CD** pipelines
- **Container** registry
- **Issue** boards
- **Security** scanning

#### Bitbucket:

- **Pipelines** for CI/CD
- **Jira** integration
- **Deployments** tracking

---

### Best Practices

#### Repository setup:

- **Use .gitignore** from the start
- **Add README.md** with project info
- **Choose** appropriate license
- **Set up** branch protection

#### Remote management:

- **Use SSH keys** for frequent access
- **Keep origin** as main remote
- **Descriptive** remote names
- **Regular** synchronization

---

### Troubleshooting Remote Issues

#### Authentication problems:

```bash
# Check remote URL
git remote -v

# Update to use SSH
git remote set-url origin git@github.com:user/repo.git

# Or use personal access token
git remote set-url origin https://token@github.com/user/repo.git
```

#### Push rejected:

```bash
git pull origin main  # Get latest changes
# resolve conflicts if any
git push origin main
```

---

### Security Considerations

#### Never commit:

- **Passwords** or API keys
- **Private keys** or certificates
- **Database** credentials
- **Environment** variables with secrets

#### Use:

- **.gitignore** for sensitive files
- **Environment variables** for secrets
- **SSH keys** for authentication
- **Personal access tokens** instead of passwords

---

### Questions?

**Understanding local vs remote is crucial for collaboration**

**Next:** Hands-on practice with remotes and hosting platforms

**Any questions about distributed Git or hosting platforms?**
