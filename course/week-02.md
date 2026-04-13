# Week 2 — Git Basics: The Edit → Stage → Commit → Push Loop

**Estimated time:** 60–90 minutes
**Goal:** Make a change to `index.html`, commit it, and see it on GitHub.

---

## What you will learn

- The four stages of a Git change: working directory → staging area → local commit → remote
- How to use `git add`, `git commit`, `git push`
- What `.gitignore` is and how to set it up

---

## Theory (10 min)

Every change you make goes through four stages:

```
[Working directory]  →  git add  →  [Staging area]  →  git commit  →  [Local repo]  →  git push  →  [GitHub]
    You edit files                  Changes selected               Snapshot saved               Online copy updated
```

Why staging? Because it lets you commit only *some* of your changes, not everything at once. You can prepare a clean, logical commit even if you edited multiple unrelated things.

**Commits are snapshots, not diffs.**
Each commit records the state of every tracked file at that moment. Git figures out the diff automatically.

---

## Tasks

### 1. Customize index.html

Open `index.html` in your editor. Find the placeholder content (name, title, description) and replace it with your own information.

You don't need to make it perfect — any visible change is enough for this exercise.

### 2. Check what changed

```bash
git status
```

You will see `index.html` listed under "Changes not staged for commit" — it is modified but not yet staged.

```bash
git diff
```

This shows the exact lines added (green `+`) and removed (red `-`).

### 3. Stage your changes

```bash
git add index.html
```

Run `git status` again — `index.html` is now under "Changes to be committed".

### 4. Commit with a meaningful message

```bash
git commit -m "Update index.html with personal info"
```

Commit message rules:
- Use present tense: "Add", "Update", "Fix" — not "Added" or "Fixed"
- Keep it under 72 characters
- Describe *what* changed, not *how*

### 5. Push to GitHub

```bash
git push
```

Open your GitHub fork in the browser. You should see your commit message next to `index.html`.

### 6. Set up .gitignore

Some files should never be committed: OS files, editor configs, etc.

Create a `.gitignore` file in the root of your project:

```
# macOS
.DS_Store

# Windows
Thumbs.db
desktop.ini

# Editor
.vscode/
.idea/

# Node (if you ever use npm locally)
node_modules/
```

Stage and commit it:

```bash
git add .gitignore
git commit -m "Add .gitignore"
git push
```

---

## Expected result

- [ ] `index.html` updated with your own content
- [ ] Change committed with a meaningful message
- [ ] Commit visible on GitHub
- [ ] `.gitignore` file in the repo

---

## Commit your work

Both commits were made during the tasks above. Verify everything is pushed:

```bash
git status
```

Expected output:
```
On branch main
nothing to commit, working tree clean
```

And confirm your commits are on GitHub:

```bash
git log --oneline
```

You should see at least 3 commits (original + `index.html` update + `.gitignore`).

---

## Checkpoint

```bash
git log --oneline
```

You should see at least 2 commits (the original + your new one).

Go to your GitHub repo → click on `index.html` → click **History** — your commit should appear there.

---

## Key commands recap

| Command | What it does |
|---------|-------------|
| `git status` | Show what changed since last commit |
| `git diff` | Show exact line-level changes |
| `git add <file>` | Stage a specific file |
| `git add .` | Stage all changed files |
| `git commit -m "message"` | Save staged changes as a commit |
| `git push` | Upload local commits to GitHub |
| `git pull` | Download remote commits to local |

---

> Next week: instead of committing directly to `main`, you will create a separate branch and open a Pull Request — the standard way professionals work.
