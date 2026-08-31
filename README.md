# Git Course - 2 Days

A comprehensive 2-day Git training course covering fundamental to intermediate Git concepts and workflows.

## Course Overview

This course is designed to take participants from Git beginners to confident intermediate users over two intensive days. The course combines theoretical concepts with hands-on exercises to ensure practical understanding.

### Day 1: Introduction to Git, Tools, and Collaboration Basics

- Course introduction and Git fundamentals
- Installation, configuration, and basic commands
- Local workflow and repository management
- Remote repositories and GitHub integration

### Day 2: Intermediate Git – Conflicts, Collaboration, & Version Tracking

- Advanced branching and merging strategies
- Conflict resolution techniques
- Intermediate commands and recovery scenarios
- Best practices and troubleshooting

## Viewing the Course Slides

This course uses [reveal-md](https://github.com/webpro/reveal-md) to create and display interactive HTML presentations from Markdown files.

### Prerequisites

- [Node.js](https://nodejs.org/) (version 14 or higher)
- npm (comes with Node.js)

### Installation

#### Option 1: Global Installation (Recommended)

```bash
npm install -g reveal-md
```

#### Option 2: Local Installation

```bash
npm install reveal-md
```

### Usage

#### Viewing All Slides

To view the main course slides that include all sections:

```bash
reveal-md slides.md
```

#### Custom Configuration

The course includes a `reveal-md.json` configuration file that customizes the presentation appearance and behavior. This will be automatically loaded when you run reveal-md from the project directory.

#### Additional Options

**Specify a different port:**

```bash
reveal-md slides.md --port 8080
```

**Disable auto-opening in browser:**

```bash
reveal-md slides.md --disable-auto-open
```

**Export to PDF:**

```bash
reveal-md slides.md --print slides.pdf
```

**Export to static HTML:**

```bash
reveal-md slides.md --static _site
```

### Navigation Controls

Once the slides are running in your browser:

- **Arrow keys**: Navigate between slides
- **Space bar**: Next slide
- **Shift + Space**: Previous slide
- **ESC**: Overview mode
- **S**: Speaker notes (if available)
- **F**: Full screen mode
- **?**: Help menu

## For Participants

### Prerequisites for Hands-on Exercises

- Git installed on your machine
- Text editor of choice
- GitHub account (free)
- Terminal/command line access

## Troubleshooting

### Common Issues

**reveal-md command not found:**

- Ensure Node.js is installed
- For global installation, make sure npm global bin directory is in your PATH
- Try using `npx reveal-md` instead

**Port already in use:**

- Use a different port: `reveal-md slides.md --port 8080`
- Or stop other processes using the default port (1948)

**Slides not loading properly:**

- Check that you're in the correct directory
- Ensure all referenced files exist
- Check browser console for errors

**Custom styles not applying:**

- Verify `custom-styles.css` exists
- Check `reveal-md.json` configuration
- Clear browser cache

### Getting Help

- [reveal-md documentation](https://github.com/webpro/reveal-md)
- [Reveal.js documentation](https://revealjs.com/)
- Course-specific questions: Contact your instructor

---

**Happy Learning!** 🚀
