# Week 5 — GitHub Actions: Your First CI Workflow

**Estimated time:** 90–120 minutes
**Goal:** Write a GitHub Actions workflow that validates your HTML on every push. See a red failure, fix it, see a green checkmark.

---

## What you will learn

- What Continuous Integration (CI) is and why it matters
- How GitHub Actions workflows work (YAML syntax basics)
- How to use `htmlhint` to lint HTML
- How to intentionally break and fix a CI check

---

## Theory (15 min)

**Continuous Integration (CI)** = automatically running checks on every code change before it is merged.

Without CI: you push code → it deploys → you find out it was broken when users complain.
With CI: you push code → automated checks run → you find out it is broken before it deploys.

**GitHub Actions** is GitHub's built-in CI/CD system. You define **workflows** in YAML files stored in `.github/workflows/`. A workflow consists of:

```
Workflow
└── Job(s)           (runs on a virtual machine)
    └── Step(s)      (individual commands or pre-built actions)
```

When you push to GitHub, Actions spins up a fresh virtual machine, runs your steps, and reports pass/fail with a ✅ or ❌ next to your commit.

**YAML basics** you will need:
```yaml
key: value               # string
number: 42               # number
list:                    # list
  - item1
  - item2
nested:                  # nested object
  key: value
```

Indentation matters — use 2 spaces, never tabs.

---

## Tasks

### 1. Create the workflow directory

```bash
mkdir -p .github/workflows
```

### 2. Create the CI workflow file

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]

jobs:
  lint-html:
    name: Lint HTML
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: Install htmlhint
        run: npm install -g htmlhint

      - name: Run HTMLHint
        run: htmlhint index.html
```

### 3. Verify the HTMLHint config file

The `.htmlhintrc` file already exists in your forked repository — it was part of the course template. Verify its contents match exactly:

```bash
cat .htmlhintrc
```

Expected output:
```json
{
  "doctype-first": true,
  "title-require": true,
  "attr-value-double-quotes": true,
  "tag-pair": true,
  "spec-char-escape": true,
  "id-unique": true,
  "src-not-empty": true,
  "attr-no-duplication": true
}
```

If the contents match — no action needed, move on to step 4.

If the file is missing or different, create/overwrite it with the content above, then stage it:
```bash
git add .htmlhintrc
```

### 4. Commit and push

```bash
git add .github/workflows/ci.yml
git commit -m "Add HTML lint CI workflow"
git push
```

### 5. Watch the workflow run

1. Go to your GitHub repository
2. Click the **Actions** tab
3. You should see your workflow running (yellow spinner) or completed (green ✅)
4. Click on it to see the logs for each step

### 6. Intentionally break HTML and see the red failure

Create a branch:

```bash
git checkout -b test/break-html
```

Open `index.html` and add a broken tag — for example, an unclosed div or a duplicate `id`:

```html
<!-- Add this somewhere in the body — it has a duplicate id (if "header" already exists) -->
<div id="header">Oops, duplicate id</div>
```

Or simply remove the `<!DOCTYPE html>` declaration from the first line.

Commit and push:

```bash
git add index.html
git commit -m "Intentionally break HTML for CI demo"
git push -u origin test/break-html
```

Open a Pull Request from this branch to `main`. Watch the CI workflow run and **fail** with a ❌.

### 7. Fix the broken HTML

Fix the issue in `index.html`:

```bash
# Fix the HTML you broke
git add index.html
git commit -m "Fix HTML lint errors"
git push
```

Watch the PR — the CI check turns green ✅.

### 8. Merge (or close) the PR

You can merge it or just close it — the important thing was seeing the red-to-green cycle.

---

## Commit your work

The `ci.yml` workflow file was committed and pushed in Task 4. The test branch from Tasks 6–8 should be merged or closed.

Verify final state on `main`:

```bash
git checkout main
git pull
git status
```

Expected output:
```
On branch main
nothing to commit, working tree clean
```

New files added this week:
- `.github/workflows/ci.yml` — your first CI workflow

---

## Expected result

- [ ] `.github/workflows/ci.yml` committed and visible in the Actions tab
- [ ] First workflow run: green ✅
- [ ] Intentionally broken HTML: CI fails ❌
- [ ] Fixed HTML: CI passes ✅
- [ ] You understand what each section of the YAML does

---

## Anatomy of the workflow (explained)

```yaml
on:                         # when to run
  push:
    branches: ["main"]      # on every push to main
  pull_request:
    branches: ["main"]      # on every PR targeting main

jobs:
  lint-html:
    runs-on: ubuntu-latest  # use a fresh Ubuntu VM

    steps:
      - uses: actions/checkout@v4    # download your repo onto the VM
      - uses: actions/setup-node@v4  # install Node.js
      - run: npm install -g htmlhint # install the linter
      - run: htmlhint index.html     # run the linter
```

If `htmlhint` exits with an error code → the step fails → the job fails → the workflow fails → ❌.

---

> Next week: you will protect the `main` branch so that no one (including you) can push directly to it — all changes must go through a PR with passing CI.
