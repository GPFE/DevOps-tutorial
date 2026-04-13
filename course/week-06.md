# Week 6 — Branch Protection & Cloudflare Preview Deployments

**Estimated time:** 60–90 minutes
**Goal:** Protect the `main` branch so direct pushes are blocked, and get a unique preview URL for every Pull Request.

**Prerequisites:** Complete [Week 4](week-04.md) (Cloudflare Pages connected) and [Week 5](week-05.md) (CI workflow committed to `main`) before starting this week.

---

## What you will learn

- How to configure branch protection rules on GitHub
- Why protecting `main` is a professional standard
- How Cloudflare Pages automatically creates preview deployments for PRs

---

## Theory (10 min)

### Branch protection

By default, anyone with write access to your repo can push directly to `main` — bypassing CI, bypassing review. Branch protection rules prevent that.

With branch protection enabled on `main`:
- Direct pushes to `main` are **rejected**
- All changes must go through a **Pull Request**
- You can require CI checks to **pass** before merging
- You can require a code **review** before merging

This is standard practice in any professional team.

### Preview deployments

When you connected your GitHub repo to Cloudflare Pages in Week 4, Cloudflare automatically authorized itself to watch your repository via a GitHub App. As part of that, preview deployments were enabled by default.

The rule is simple: **every branch that is not your production branch (`main`) automatically gets its own preview deployment** when you push to it. Each PR gets a unique URL like:

```
https://abc123.devops-yourname.pages.dev
```

Cloudflare also posts this URL as a comment directly on the GitHub PR, so reviewers can click straight to the preview without going to the dashboard.

This means reviewers can visually test a change **before** it merges to `main`. No more "it looks different in production" surprises.

---

## Tasks

### 1. Enable branch protection on `main`

1. Go to your GitHub repository
2. Click **Settings** → **Branches**
3. Under **Branch protection rules**, click **Add rule**
4. Set **Branch name pattern** to `main`
5. Enable these options:
   - ✅ **Require a pull request before merging**
     - Set "Required approvals" to `0` (you are working solo — you can merge your own PRs)
   - ✅ **Require status checks to pass before merging**
     - Click **Search for status checks** and add `Lint HTML` (the job name from your CI workflow)
     - ✅ Check **Require branches to be up to date before merging**
   - ✅ **Do not allow bypassing the above settings**
6. Click **Create**

### 2. Test that direct push is blocked

Try pushing directly to main:

```bash
# Make a small change to any file
echo "<!-- test -->" >> index.html
git add index.html
git commit -m "Test direct push (should be blocked)"
git push
```

You should see an error like:
```
remote: error: GH006: Protected branch update failed for refs/heads/main.
remote: error: Required status check "Lint HTML" is expected.
```

Revert the test change:

```bash
git reset --soft HEAD~1          # undo the commit (keep changes staged)
git rm --cached index.html       # unstage
git checkout -- index.html       # discard the change
```

> `git checkout -- <file>` works on all Git versions. If your Git is 2.23 or newer you can also use `git restore --staged index.html` followed by `git restore index.html` — both do the same thing.

### 3. Check preview deployment settings in Cloudflare dashboard

Before testing, confirm the setting is active:

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages**
2. Click your Pages project (e.g. `devops-yourname`)
3. Click **Build** in the top navigation
4. Find the **Branch control** section
5. Under **Preview branches**, confirm the setting is **All non-production branches**

This is the default — if it says anything else, change it to **All non-production branches** and save.

> **Why does Cloudflare even see your PRs?** When you clicked "Connect GitHub" in Week 4, Cloudflare installed a GitHub App on your repository in the background. That app is what lets Cloudflare watch your branches, trigger builds, and post preview URLs as PR comments.

### 4. Test preview deployments

1. Create a branch:
   ```bash
   git checkout -b feature/test-preview
   ```

2. Make a visible change to `index.html` — change a heading, color, or any text.

3. Commit and push:
   ```bash
   git add index.html
   git commit -m "Test preview deployment"
   git push -u origin feature/test-preview
   ```

4. Open a Pull Request on GitHub.

5. Wait ~30–60 seconds. Cloudflare Pages will post a comment on the PR:
   ```
   ✅ Deploy Preview for devops-yourname ready!
   Preview URL: https://abc123.devops-yourname.pages.dev
   ```

6. Click the preview URL — you should see your change live, without it having merged to `main`.

   If no comment appears after 2 minutes:
   - Go to your Cloudflare Pages dashboard → **Deployments** tab — check if a build was triggered at all
   - If no build: go back to **Build → Branch control** and confirm preview branches are set to **All non-production branches**
   - If a build ran but no PR comment appeared: the GitHub App authorization may need refreshing — go to your Cloudflare Pages project → **Settings** → **Git repository** → reconnect

### 5. Merge the PR

Since `Lint HTML` CI must pass before you can merge:
1. Wait for the green ✅ on the CI check
2. Click **Merge pull request**
3. Notice the Merge button was **disabled** until CI passed

---

## Commit your work

Your `index.html` change from Task 3 was committed on the `feature/test-preview` branch and merged into `main` via PR. After merging, update your local `main`:

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

No new config files were added this week — branch protection is configured entirely in GitHub Settings.

---

## Expected result

- [ ] Branch protection rules active on `main`
- [ ] Direct push to `main` is rejected with an error
- [ ] PR has a Cloudflare Pages preview URL
- [ ] PR can only be merged after CI passes

---

## What the full flow looks like now

```
git push (feature branch)
        ↓
GitHub Actions runs HTML lint
        ↓
Cloudflare Pages builds a preview URL
        ↓
PR shows: ✅ CI passed  +  🔗 Preview URL
        ↓
You merge
        ↓
Cloudflare Pages deploys to production URL
```

---

## Checkpoint

Go to your repo's **Settings → Branches** — you should see `main` listed with protection rules. The lock icon confirms it is protected.

---

> Next week: you will add an AI assistant (Claude via Azure AI Foundry) to your pipeline. It will automatically review every PR and leave comments with suggestions.
