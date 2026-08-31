# Git course

<div style="display: flex; flex-direction: row; gap: 50px; align-items: center; justify-content: center;">
<img src="https://www.scidetech.com/_next/image?url=%2Fscidetech-logo.png&w=384&q=75" alt="Scidetech logo" style="width: 300px;">
<img src="https://cdn.prod.website-files.com/6616342debb1b03d50003b90/67765ebc8985f3df0f5fc3e6_Edument.svg" alt="edument logo" style="width:300px;"/>
</div>
---

## What is Version Control?

_Managing changes to files over time_

---
  
## The Problem Without Version Control

- `final_report.doc`
- `final_report_v2.doc`
- `final_report_v2_FINAL.doc`
- `final_report_v2_FINAL_REALLY.doc`
- `final_report_v2_FINAL_REALLY_USE_THIS_ONE.doc`

**Sound familiar?** 😅

---

## The case of Knight Capital

### a $440 million dollar mistake.

![knight](/resources/img/knight-capital-group.png.webp)

[Case Study -- click here](https://www.henricodolfing.ch/en/case-study-4-the-440-million-software-error-at-knight-capital/)

---

## What is Version Control?

A system that records changes to files over time so you can:

- **Track changes** - See what changed, when, and who changed it
- **Revert files** - Go back to previous versions
- **Compare versions** - See differences between file versions
- **Collaborate** - Multiple people working on the same files
- **Backup** - Distributed copies of your work

---

## Version Control Benefits

### For Individuals

- Never lose work again
- Experiment safely with branches
- Track your progress over time

### For Teams

- Collaborate without conflicts
- See who changed what and when
- Merge work from multiple contributors

---

## Types of Version Control Systems

---

### Types of Version Control Systems

- Local Version Control
- Centralized Version Control
- Distributed Version Control

---

### Local Version Control

_Copy files to another directory_

```
MyProject/
├── version1/
├── version2/
├── version3/
└── current/
```

**Problems:** Error-prone, no collaboration, easy to mess up

---

### Centralized Version Control

_Single server, multiple clients_

```
    Central Server
         ┃
    ┏━━━━╋━━━━┓
    ┃    ┃    ┃
 Client Client Client
```

**Examples:** CVS, Subversion (SVN), Perforce

---

### Centralized VCS - Pros & Cons

**Pros:**

- Everyone knows what others are doing
- Administrators have fine-grained control
- Easier to manage than local VCS

**Cons:**

- Single point of failure
- No work when server is down
- Network dependency

---

### Distributed Version Control

_Every client has full history_

```
 Repository  Repository  Repository
     ┃           ┃           ┃
   Client ←――→ Client ←――→ Client
```

**Examples:** Git, Mercurial, Bazaar

---

### Distributed VCS - Advantages

- **No single point of failure**
- **Work offline**
- **Multiple backup copies**
- **Flexible workflows**
- **Fast operations** (most operations are local)

---

## Version Control Options Comparison

---

### Git

- **Distributed**
- **Fast and efficient**
- **Excellent branching/merging**
- **Large community**
- **GitHub, GitLab, Bitbucket**

<img src="https://brandeps.com/logo-download/G/Git-logo-vector-01.svg" style="display: block;width:200px; margin:auto;"/>

---

### Subversion (SVN)

- **Centralized**
- **Simpler mental model**
- **Good for binary files**
- **Enterprise adoption**
- **Linear history**

<img src="https://svn.apache.org/repos/asf/subversion/svn-logos/images/tyrus-svn2.png" style="display: block;width:200px; margin:auto;"/>

---

### Mercurial

- **Distributed**
- **Python-based**
- **Simpler than Git**
- **Good performance**
- **Less popular**

<img src="https://www.logo.wine/a/logo/Mercurial/Mercurial-Logo.wine.svg" style="display: block;width:400px; margin:auto;"/>

---

### Perforce

- **Centralized**
- **Enterprise-focused**
- **Great for large files**
- **Expensive licensing**
- **Game development popular**

<img src="https://www.perforce.com/themes/custom/p4base/assets/images/logo.svg" style="display: block;width:400px; margin:auto;" />

---

## Why Choose git?

<img src="https://brandeps.com/logo-download/G/Git-logo-vector-01.svg" style="display: block;width:200px; margin:auto;"/>

---

### Git's Key Advantages

1. **Performance** - Blazingly fast
2. **Distributed** - No single point of failure
3. **Branching** - Cheap and easy
4. **GitHub Effect** - Massive adoption
5. **Open Source** - Free and community-driven
6. **Flexibility** - Supports many workflows

---

### The GitHub Effect

- **2008:** GitHub launched
- **Social coding** - Fork, star, follow
- **Free hosting** for open source
- **Issue tracking** and project management
- **Pull requests** revolutionized collaboration

**Result:** Git became the de facto standard

---

### Git Adoption Statistics

- **90%+** of developers use Git (Stack Overflow Survey)
- **GitHub:** 100M+ repositories
- **Major companies:** Google, Microsoft, Facebook, Netflix
- **Open source:** Linux kernel, React, VS Code

---

## Git vs The Competition

<br>

| Feature        | Git    | SVN    | Mercurial | Perforce |
| -------------- | ------ | ------ | --------- | -------- |
| Distributed    | ✅     | ❌     | ✅        | ❌       |
| Speed          | ⭐⭐⭐ | ⭐⭐   | ⭐⭐⭐    | ⭐⭐     |
| Branching      | ⭐⭐⭐ | ⭐     | ⭐⭐      | ⭐⭐     |
| Learning Curve | ⭐     | ⭐⭐⭐ | ⭐⭐      | ⭐⭐     |
| Community      | ⭐⭐⭐ | ⭐⭐   | ⭐        | ⭐       |

<br>

---

## When NOT to Use Git?

- **Very large binary files** (consider Git LFS)
- **Extremely simple projects** (maybe overkill)
- **Legacy systems** tied to other VCS
- **Specific enterprise requirements**

_But honestly... just use Git_ 😉

---

## Real-World Git Usage

### Open Source Projects

- Linux Kernel (where Git was born)
- React, Vue, Angular
- VS Code, Atom
- Docker, Kubernetes

---

## Real-World Git Usage

### Companies

- Microsoft moved from Perforce
- Google uses Git extensively
- Facebook, Netflix, Airbnb
- Virtually every tech company

---

## Getting Started with Git

### What You'll Learn Today:

1. **Installing and configuring Git**
2. **Basic Git workflow**
3. **Working with remotes**
4. **Best practices and tips**

---

## Questions?

_Ready to dive into Git?_

**Next:** Installing and Configuring Git
