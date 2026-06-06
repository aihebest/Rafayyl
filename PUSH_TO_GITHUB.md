# 🚀 Deploy Rafayyl Engineering to GitHub + Vercel
## Step-by-Step Guide

---

## STEP 1 — Install Git (if not already installed)
Download from https://git-scm.com/downloads and install with defaults.

---

## STEP 2 — Open Terminal / Command Prompt
On Windows: Press `Win + R` → type `cmd` → Enter
Or use VS Code terminal (View → Terminal)

---

## STEP 3 — Navigate to the project folder
```bash
cd "C:\Users\USER\OneDrive - Desicon Group\Documents\Claude\Projects\Rafayyl Engineering"
```

---

## STEP 4 — Initialize Git and push to GitHub
Run these commands **one by one**:

```bash
# Initialize the repo
git init

# Set your identity (use your GitHub email)
git config user.email "aihebest@gmail.com"
git config user.name "Rafayyl Engineering"

# Add the remote (your GitHub repo)
git remote add origin https://github.com/aihebest/Rafayyl.git

# Stage all files
git add .

# First commit
git commit -m "🚀 Initial production deployment — Rafayyl Engineering website"

# Set main branch and push
git branch -M main
git push -u origin main
```

> If prompted for GitHub credentials, use your GitHub username and a
> **Personal Access Token** (not your password).
> Generate one at: https://github.com/settings/tokens → New token →
> check "repo" scope → copy the token → paste as password.

---

## STEP 5 — Connect Vercel to GitHub

1. Go to https://vercel.com → **Sign in with GitHub**
2. Click **"Add New → Project"**
3. Find **"Rafayyl"** in your GitHub repos → click **Import**
4. Vercel auto-detects it as a static site — no changes needed
5. Set **Root Directory** to `./` (default)
6. Click **"Deploy"** → wait ~30 seconds

✅ Your site is now live on a Vercel URL like `rafayyl.vercel.app`

---

## STEP 6 — Add GitHub Secrets for CI/CD (Auto-deploy)

This enables automatic deployment whenever you push to GitHub.

### Get your Vercel tokens:
```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Link project (run from project folder)
cd "C:\Users\USER\OneDrive - Desicon Group\Documents\Claude\Projects\Rafayyl Engineering"
vercel link
```
After `vercel link`, check the `.vercel/project.json` file — it contains:
- `projectId` → your VERCEL_PROJECT_ID
- `orgId` → your VERCEL_ORG_ID

Get your VERCEL_TOKEN: https://vercel.com/account/tokens → Create Token

### Add secrets to GitHub:
1. Go to https://github.com/aihebest/Rafayyl/settings/secrets/actions
2. Click **"New repository secret"** for each:

| Secret Name        | Value                          |
|--------------------|-------------------------------|
| `VERCEL_TOKEN`     | Your Vercel personal token    |
| `VERCEL_ORG_ID`    | From `.vercel/project.json`   |
| `VERCEL_PROJECT_ID`| From `.vercel/project.json`   |

✅ Now every `git push` to main = automatic deployment!

---

## STEP 7 — Connect Custom Domain (rafayyl.com)

### In Vercel:
1. Project dashboard → **Settings → Domains**
2. Add domain: `rafayyl.com`
3. Also add: `www.rafayyl.com`
4. Vercel shows you DNS records to add

### In Microsoft (your domain host):
Since your domain is hosted via Microsoft, go to:
**Microsoft 365 Admin Center → Settings → Domains → rafayyl.com → DNS records**

Add these records:

| Type  | Name | Value                    | TTL  |
|-------|------|--------------------------|------|
| A     | @    | `76.76.21.21`            | 3600 |
| CNAME | www  | `cname.vercel-dns.com`   | 3600 |

> DNS changes take 5–60 minutes to propagate.

✅ After propagation, https://rafayyl.com serves your production site with free SSL.

---

## STEP 8 — Activate FormSubmit (Contact Form)

1. After the site goes live, fill in and submit the **Contact form** once
2. Check **info@rafayyl.com** inbox for a confirmation email from FormSubmit
3. Click the activation link
4. All future form submissions will now arrive at info@rafayyl.com ✅

---

## STEP 9 — Set Up Google Analytics 4

1. Go to https://analytics.google.com
2. Create account → Property name: "Rafayyl Engineering"
3. Platform: Web → URL: `rafayyl.com`
4. Copy your **Measurement ID** (format: `G-XXXXXXXXXX`)
5. In `index.html`, find (Ctrl+F):
   ```
   G-XXXXXXXXXX
   ```
   Replace both occurrences with your real ID
6. Commit and push:
   ```bash
   git add index.html
   git commit -m "feat: add GA4 measurement ID"
   git push
   ```
Vercel auto-deploys in ~20 seconds ✅

---

## STEP 10 — Admin Panel

- URL: `https://rafayyl.com/admin`
- Default login: `admin` / `rafayyl2024`
- **CHANGE PASSWORD IMMEDIATELY** → Settings → Change Password

---

## Making Future Updates

Any time you edit files, just run:
```bash
git add .
git commit -m "describe what you changed"
git push
```
Vercel deploys automatically in ~20 seconds. ✅

---

## Quick Reference

| Item                  | Value                                     |
|-----------------------|-------------------------------------------|
| GitHub Repo           | https://github.com/aihebest/Rafayyl.git   |
| Live Site             | https://rafayyl.com                       |
| Admin Panel           | https://rafayyl.com/admin                 |
| Contact Email         | info@rafayyl.com                          |
| WhatsApp              | +234 705 469 9267                         |
| Gmail (social)        | rafayyleng@gmail.com                      |
| LinkedIn              | linkedin.com/company/rafayyleng           |
| X / Twitter           | twitter.com/rafayyleng                    |
| Facebook              | facebook.com/rafayyleng                   |
| Instagram             | instagram.com/rafayyleng                  |

