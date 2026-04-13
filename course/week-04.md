# Week 4 — Cloudflare Pages: Your First Live Deploy

**Estimated time:** 45–60 minutes
**Goal:** Connect your GitHub repository to Cloudflare Pages and see your site live on a public URL.

---

## What you will learn

- What static hosting is and why Cloudflare Pages is a good choice
- How to connect a GitHub repo to Cloudflare Pages via the UI
- How continuous deployment works: every push = new version live

---

## Theory (10 min)

**Static hosting** = serving HTML, CSS, and JS files directly from a CDN (Content Delivery Network), with no server-side code. Perfect for HTML5 apps.

**Cloudflare Pages** gives you:
- Free hosting on Cloudflare's global CDN
- Automatic HTTPS
- Every push to `main` triggers a new deploy automatically
- Each Pull Request gets its own preview URL (we will use this in Week 6)

The connection is simple: Cloudflare watches your GitHub repo. When it sees a new commit on `main`, it pulls the files and serves them. No configuration files needed for a plain HTML project.

---

## Tasks

### 1. Create a Cloudflare account

Go to [dash.cloudflare.com/sign-up](https://dash.cloudflare.com/sign-up) and sign up with your email.
No credit card required.

### 2. Create a new Pages project

1. In the Cloudflare dashboard, go to **Workers & Pages** → **Pages**
2. Click **Create a project** → **Connect to Git**
3. Click **Connect GitHub** and authorize Cloudflare to access your repositories
4. Select your forked `DevOps` repository
5. Click **Begin setup**

### 3. Configure build settings

For a plain HTML project (no build step needed):

| Setting | Value |
|---------|-------|
| Project name | `devops-yourname` (or anything you like) |
| Production branch | `main` |
| Framework preset | `None` |
| Build command | leave blank — delete any pre-filled value |
| Build output directory | leave blank — delete any pre-filled value |

Click **Save and Deploy**.

### 4. Wait for the first deploy

Cloudflare will pull your repo and deploy it. This takes about 30–60 seconds.

When it says **Success**, click the generated URL — something like:
`https://devops-yourname.pages.dev`

Your site is live.

### 5. Trigger a deploy by pushing

Make a small change to `index.html`:

```bash
# Edit index.html — change the page title or any text
git add index.html
git commit -m "Test automatic deploy"
git push
```

Go to your Cloudflare Pages dashboard → **Deployments** — watch the new deploy appear automatically.
Refresh your `pages.dev` URL after ~30 seconds — you will see the updated version.

---

## Expected result

- [ ] Cloudflare Pages account created
- [ ] GitHub repo connected to Cloudflare Pages
- [ ] Site live at `https://your-project.pages.dev`
- [ ] Push to `main` triggers an automatic deploy

---

## Commit your work

The test commit from Task 5 is already pushed. Verify the final state:

```bash
git status
```

Expected output:
```
On branch main
nothing to commit, working tree clean
```

No new files were added this week — Cloudflare Pages is configured entirely through the UI and stores nothing in the repository.

---

## Checkpoint

1. Open your `pages.dev` URL in an incognito browser window
2. You should see your customized `index.html`
3. In Cloudflare dashboard → **Deployments**, you should see 2 deployments (initial + your test push)

---

## What just happened (the DevOps story so far)

```
You push to GitHub
       ↓
Cloudflare Pages detects the push (via webhook)
       ↓
Cloudflare pulls your files
       ↓
Files served on global CDN
       ↓
Your site is live in ~30 seconds
```

This is **Continuous Deployment (CD)** — code changes automatically flow to production. You didn't click "upload" or "deploy" — it just happened.

---

> Next week: before code reaches Cloudflare, we will add an automated check that validates your HTML. If the check fails, you will know immediately — before it goes live.
