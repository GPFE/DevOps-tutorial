# Week 3 — Branches & Pull Requests

**Estimated time:** 60–90 minutes
**Goal:** Make a change on a feature branch and merge it into `main` via a Pull Request.

---

## What you will learn

- What a branch is and why we use them
- How to create, switch, and merge branches
- How to open and merge a Pull Request on GitHub

---

## Theory (10 min)

A **branch** is an independent line of development. Think of it as a parallel copy of your code where you can experiment without touching the stable version.

```
main ──────────────────────────────●── (stable, live code)
          \                       /
           ● ── ● ── ●          merge
        feature/update-hero   (your work)
```

**Why not commit directly to `main`?**
- On a team, multiple people work at the same time — branches prevent conflicts
- `main` stays deployable at all times — broken work-in-progress stays on its own branch
- Pull Requests give a chance to review code before it reaches `main`

**Pull Request (PR)** = a request to merge your branch into another branch. It is a conversation: it shows the diff, allows comments, and requires approval before merging.

Even when working alone, PRs are good practice — and in Week 6 you will add automated checks that run on every PR.

---

## Tasks

### 1. Create a feature branch

```bash
git checkout -b feature/update-style
```

`-b` means "create and switch to this new branch". The branch name should describe what you are working on.

Verify you are on the new branch:

```bash
git branch
```

The current branch has a `*` next to it.

### 2. Make a change

Open `style.css` and change something visible — the background color, font, accent color. Anything you can see in the browser.

### 3. Commit on the branch

```bash
git add style.css
git commit -m "Update color scheme"
```

Note: this commit exists **only on your feature branch**. Switch back to `main` with `git checkout main` — `style.css` is unchanged there. That is the whole point of branches.

Switch back to your feature branch to continue:

```bash
git checkout feature/update-style
```

### 4. Push the branch to GitHub

```bash
git push -u origin feature/update-style
```

`-u origin feature/update-style` sets the upstream — next time you only need `git push`.

### 5. Open a Pull Request on GitHub

1. Go to your GitHub repository
2. GitHub will show a yellow banner: **"Compare & pull request"** — click it
3. Set:
   - **base:** `main`
   - **compare:** `feature/update-style`
4. Write a short description of what you changed
5. Click **Create pull request**

### 6. Review and merge

1. Look at the **Files changed** tab — this is the diff
2. Click **Merge pull request** → **Confirm merge**
3. Click **Delete branch** (good hygiene — keep the branch list clean)

### 7. Update your local main

After merging on GitHub, your local `main` is behind. Fix that:

```bash
git checkout main
git pull
```

---

## Expected result

- [ ] Created a feature branch
- [ ] Made a visual change to `style.css`
- [ ] Opened a Pull Request on GitHub
- [ ] Merged the PR
- [ ] Local `main` is up to date

---

## Commit your work

Your commit was made on the feature branch and merged into `main` via a PR. After running `git pull` in Task 7, verify the final state:

```bash
git status
```

Expected output:
```
On branch main
nothing to commit, working tree clean
```

Also verify you are on `main` (not still on the feature branch):

```bash
git branch
```

The `*` should be next to `main`. If you are still on `feature/update-style`, run `git checkout main`.

---

## Checkpoint

```bash
git log --oneline main
```

You should see the merge commit (or the squashed commit) from your PR at the top.

---

## Key commands recap

| Command | What it does |
|---------|-------------|
| `git checkout -b <name>` | Create and switch to new branch |
| `git checkout <name>` | Switch to existing branch |
| `git branch` | List all local branches |
| `git push -u origin <name>` | Push branch to GitHub (first time) |
| `git pull` | Download and merge remote changes |

---

## Branch naming conventions

Good branch names are short, lowercase, and use hyphens:

- `feature/add-contact-form`
- `fix/broken-nav-link`
- `chore/update-gitignore`

---

> Next week: you will connect your GitHub repo to Cloudflare Pages so that every push to `main` automatically deploys your site to a public URL.
