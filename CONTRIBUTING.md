# 🤝 Contributing to Cybersecurity CTF Roadmap

> **Thank you for wanting to contribute to C!p#3r Club IIST's CTF Roadmap!** Every contribution — big or small — helps students learn cybersecurity better.

---

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Contribute](#how-to-contribute)
- [Types of Contributions](#types-of-contributions)
- [Development Setup](#development-setup)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Pull Request Process](#pull-request-process)
- [Style Guide](#style-guide)
- [First-Time Contributors](#first-time-contributors)

---

## 🤝 Code of Conduct

This project is a **safe, inclusive, beginner-friendly space**. We expect all contributors to:

- Be respectful and constructive
- Welcome people of all skill levels
- Give credit where it is due
- Focus on learning and community building
- **Never share actual malware or tools for illegal hacking**

Violations will result in your contribution being rejected or being blocked from the project.

---

## 🚀 How to Contribute

### Step-by-Step Guide

```bash
# Step 1: Fork the repository
# Go to https://github.com/CipherClubIIST/Cybersecurity-CTF-Roadmap
# Click the "Fork" button in the top right

# Step 2: Clone your fork
git clone https://github.com/YOUR-USERNAME/Cybersecurity-CTF-Roadmap.git
cd Cybersecurity-CTF-Roadmap

# Step 3: Add the upstream remote
git remote add upstream https://github.com/CipherClubIIST/Cybersecurity-CTF-Roadmap.git

# Step 4: Create a feature branch
git checkout -b feature/your-contribution-name

# Step 5: Make your changes
# (See Types of Contributions below)

# Step 6: Stage and commit
git add .
git commit -m "feat: add notes for Linux privilege escalation"

# Step 7: Push your branch
git push origin feature/your-contribution-name

# Step 8: Open a Pull Request
# Go to your fork on GitHub and click "Compare & pull request"
```

### Keeping Your Fork Updated

```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## 📝 Types of Contributions

### ✅ We Accept

| Type | Example |
|---|---|
| 📖 Learning notes | Adding notes for a phase, tool, or technique |
| 🔗 Resource links | Free courses, videos, books, cheat sheets |
| 🛠️ Tool guides | Installation and usage walkthroughs |
| 💡 Tips & tricks | CTF solving strategies |
| 🐛 Bug fixes | Fix broken links, typos, code errors |
| 🌐 Translations | Translating content to other languages |
| 🏆 CTF Writeups | Post-competition writeups (educational) |
| 📋 Checklists | Learning checklists for each phase |
| 🖥️ Code snippets | Python/Bash scripts for CTF tasks |

### ❌ We Do NOT Accept

- Actual malware samples
- Exploits targeting real production systems
- Tools designed for illegal activity
- Content that violates ethical hacking principles
- Plagiarized content without attribution

---

## 🛠️ Development Setup

No special setup needed — this is a documentation repository in Markdown. You only need:

- A text editor (VS Code recommended)
- Git installed
- A GitHub account

**Optional — Run Markdown Linter locally:**

```bash
npm install -g markdownlint-cli
markdownlint "**/*.md" --ignore node_modules
```

---

## 📝 Commit Message Guidelines

We follow the **Conventional Commits** specification:

```
<type>(<scope>): <short description>

Types:
  feat     → New content or feature
  fix      → Bug fix (broken link, typo, etc.)
  docs     → Documentation changes
  chore    → Maintenance (CI, formatting)
  refactor → Restructuring without new content

Examples:
  feat(phases): add notes for phase 3 linux fundamentals
  fix(resources): update broken TryHackMe link
  docs(readme): add web security basics section
  feat(cheatsheets): add cryptography quick reference
  fix(typo): correct spelling in networking section
```

---

## 🔄 Pull Request Process

1. **Open an issue first** for major additions (so we can discuss)
2. Fill out the PR template completely
3. Link the relevant issue in your PR description
4. Make sure your Markdown is clean and well-formatted
5. Add yourself to the Contributors section if this is your first PR
6. Wait for a maintainer review (we try to respond within 48 hours)
7. Address review feedback if requested
8. Your PR gets merged! 🎉

---

## ✏️ Style Guide

### Markdown Formatting

```markdown
# Use one H1 per file (the page title)
## Use H2 for major sections
### Use H3 for subsections

# Checklists
- [ ] Uncompleted task
- [x] Completed task

# Code blocks — ALWAYS specify the language
```bash
echo "Use language identifiers"
```python
print("Always specify")
```

# Tables — use alignment
| Column 1 | Column 2 | Column 3 |
|---|---|---|
| Data | Data | Data |

# Links
[Display Text](https://url.com)
[Internal Link](docs/phases/01-computer-basics.md)

# Emojis — use them, but sparingly and professionally
✅ Good usage — marking completed items
🎯 Good usage — highlighting important things
❌ Avoid — using too many in one line
```

### Content Guidelines

- Write in **simple, beginner-friendly English**
- Assume the reader knows **nothing** about the topic
- Give context before commands — explain what you're doing first
- Add **expected output** after commands where helpful
- Always include **free resources** — no paywalled content as primary references
- Keep sections **focused** — one topic per file

---

## 🌱 First-Time Contributors

Never contributed to open source before? No problem — we were all beginners once!

### Easy First Issues

Look for issues tagged:
- `good first issue` — Perfect for first-timers
- `help wanted` — We need community help
- `documentation` — Writing/improving notes
- `beginner-friendly` — Simple fixes

### Beginner Resources for Contributing

- [How to Fork and Make a Pull Request](https://docs.github.com/en/get-started/quickstart/fork-a-repo)
- [Git Basics — Official Docs](https://git-scm.com/doc)
- [GitHub Flow Guide](https://docs.github.com/en/get-started/quickstart/github-flow)
- [Markdown Guide](https://www.markdownguide.org/)

---

## 🏆 Recognition

All contributors will be:

- Listed in our Contributors section in README
- Credited in the specific file they contributed to
- Featured in our club announcements for major contributions

---

<div align="center">

**Questions?** Open a [GitHub Discussion](https://github.com/CipherClubIIST/Cybersecurity-CTF-Roadmap/discussions) or reach out to the maintainers.

*C!p#3r Club IIST — Decode · Defend · Dominate* 🔐

</div>
