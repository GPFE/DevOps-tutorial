# Week 1 — GitHub Setup: Fork, Clone, Explore

**Estimated time:** 60–90 minutes
**Goal:** Have the course repository forked, cloned locally, and understand what you just did.

---

## What you will learn

- What GitHub is and why developers use it
- The difference between a **repository**, a **fork**, and a **clone**
- How to navigate a repository on GitHub

---

## Before you start — install a code editor

If you do not have a code editor yet, install **Visual Studio Code** — it is free, works on all platforms, and has a built-in terminal.

Download: [code.visualstudio.com](https://code.visualstudio.com)

Install with defaults. You will use it throughout this course to edit files and run terminal commands.

---

## Theory (10 min)

**Git** is a version control system — it tracks every change you make to your code.
**GitHub** is a website that hosts Git repositories and adds collaboration features (pull requests, issues, Actions).

Three key concepts for today:

| Term | What it means |
|------|--------------|
| **Repository (repo)** | A folder tracked by Git — contains all your files + their full history |
| **Fork** | Your personal copy of someone else's repo on GitHub |
| **Clone** | Downloading a repo from GitHub to your computer |

The flow: someone creates a repo → you **fork** it (GitHub copy) → you **clone** it (local copy) → you work locally.

---

## Tasks

### 1. Create a GitHub account

Go to [github.com](https://github.com) and sign up.
- Verify your email address.

### 2. Fork this repository

1. Open the course repository on GitHub (the one you are reading this in)
2. Click the **Fork** button in the top-right corner
3. Leave all settings as default, click **Create fork**

You now have your own copy at `github.com/YOUR-USERNAME/DevOps`.

### 3. Open a terminal

All commands in this course are run in a terminal (also called command line or shell). Here is how to open one:

- **Windows:** Open **VS Code** → menu **Terminal** → **New Terminal**. A panel opens at the bottom. Alternatively, press `Win + R`, type `cmd`, press Enter.
- **macOS:** Press `Cmd + Space`, type `Terminal`, press Enter. Or open **VS Code** → **Terminal** → **New Terminal**.
- **Linux:** Press `Ctrl + Alt + T`, or open a terminal from your application menu.

> Throughout this course, all commands are shown in code blocks like this:
> ```bash
> git --version
> ```
> Type them exactly as shown (or copy-paste) and press Enter to run.

### 4. Install Git

Check if Git is already installed:

```bash
git --version
```

If you see something like `git version 2.43.0` — you are good. If you see an error:
- **Windows:** Download from [git-scm.com](https://git-scm.com/download/win), install with all defaults
- **macOS:** Run `xcode-select --install` in the terminal and follow the prompts
- **Linux:** `sudo apt install git` (Ubuntu/Debian) or `sudo dnf install git` (Fedora)

After installing, verify it works:

```bash
git --version
```

Configure Git with your name and the email you used for GitHub (one-time setup):

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### 5. Authenticate with GitHub


**Pick one option — if in doubt, choose Option A:**

---

**Option A — GitHub CLI (recommended)**

The easiest setup. One command opens a browser, you click "Authorize", done. This is the option the course assumes you have if anything goes wrong with auth later.

1. Install GitHub CLI:
   - **Windows:** `winget install --id GitHub.cli` in the terminal, or download the installer from [cli.github.com](https://cli.github.com)
   - **macOS:** `brew install gh` (requires [Homebrew](https://brew.sh)) or download from [cli.github.com](https://cli.github.com)
   - **Linux:** see [cli.github.com/manual/installation](https://cli.github.com/manual/installation)

2. Authenticate:
   ```bash
   gh auth login
   ```
   Answer the prompts:
   - **Where do you use GitHub?** → `GitHub.com`
   - **What is your preferred protocol?** → `HTTPS`
   - **Authenticate Git with your GitHub credentials?** → `Yes`
   - **How would you like to authenticate?** → `Login with a web browser`

   A code appears in the terminal. Your browser opens — paste the code, click **Authorize**.

From now on `git push` and `git pull` work without asking for credentials.

---

**Option B — Personal Access Token (PAT)**

No extra tool needed, but you need to save the token yourself.

1. Go to GitHub → your avatar → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. Click **Generate new token (classic)**
3. Set a name (e.g. `devops-course`), expiration **90 days**, check the **repo** scope
4. Click **Generate token** — copy it immediately, you will not see it again
5. Save it somewhere (password manager, secure notes)

When you run `git push` for the first time, Git asks:
- **Username:** your GitHub username
- **Password:** paste your token here (not your actual GitHub password)

On Windows, Git Credential Manager saves it after the first use. On macOS it goes into Keychain.

---

**Option C — SSH key**

More steps upfront, then zero friction forever. Good choice if you plan to use Git beyond this course.

1. Generate a key pair (use your GitHub email):
   ```bash
   ssh-keygen -t ed25519 -C "you@example.com"
   ```
   Press **Enter** to accept the default save location. You can set a passphrase or leave it empty.

2. Print the public key:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   Select and copy the entire output.

3. Add it to GitHub: **Settings** → **SSH and GPG keys** → **New SSH key** → paste → **Add SSH key**

4. Test:
   ```bash
   ssh -T git@github.com
   ```
   Expected: `Hi YOUR-USERNAME! You've successfully authenticated...`

5. When cloning, use the SSH URL (not HTTPS). On the green **Code** button, switch to the **SSH** tab to get it:
   ```bash
   git clone git@github.com:YOUR-USERNAME/DevOps.git
   ```

---

| | Option A — GitHub CLI | Option B — PAT | Option C — SSH |
|-|-----------------------|----------------|----------------|
| **Setup time** | ~2 min | ~5 min | ~10 min |
| **Ongoing friction** | None | None (saved after first use) | None |
| **Extra software** | `gh` CLI | None | None |
| **Recommended for** | Beginners ← start here | If you can't install `gh` | Long-term Git users |

---

### 6. Clone your fork

1. On your fork's GitHub page, click the green **Code** button
2. Copy the **HTTPS** URL: `https://github.com/YOUR-USERNAME/DevOps.git`
   - If you chose Option C (SSH), click the **SSH** tab and copy that URL instead
3. In your terminal, navigate to where you want to keep projects:
   ```bash
   cd ~
   ```
4. Clone:
   ```bash
   git clone https://github.com/YOUR-USERNAME/DevOps.git
   cd DevOps
   ```

### 7. Explore the repository

```bash
ls
```

You should see: `index.html`, `style.css`, `script.js`, `README.md`, `course/`

Open the project folder in VS Code:
```bash
code .
```

Spend a few minutes reading through the files:
- `index.html` — the HTML5 app you will deploy throughout this course
- `course/` — weekly assignments (you are reading one right now)

Double-click `index.html` in your file manager (or drag it into a browser) to see what it looks like.

---

## Expected result

- [ ] GitHub account created
- [ ] Repository forked to your account
- [ ] Git installed and configured
- [ ] Authentication set up (Option A, B, or C)
- [ ] Repository cloned to your computer
- [ ] You can open `index.html` in a browser and see a page

---

## Commit your work

Nothing to commit this week — you only cloned an existing repository, you did not change any files.

Verify your working tree is clean:

```bash
git status
```

Expected output:
```
On branch main
nothing to commit, working tree clean
```

If you see modified files, you accidentally changed something. Run `git diff` to see what changed.

---

## Checkpoint

```bash
git log --oneline
```

You should see at least one commit. If you do — Week 1 complete.

---

## Key terms to remember

- `git clone <url>` — download a repo to your machine
- `git log` — see the commit history
- **origin** — the name Git gives to the remote repo (your GitHub fork)

---

> Next week: you will make your first change, commit it, and push it back to GitHub.
