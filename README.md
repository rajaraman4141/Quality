# AML QC Engine — Setup Guide

## File Overview

| File | Purpose |
|------|---------|
| `index.html` | Login page (public entry point) |
| `aml_qc_scorer.html` | Main QC app (protected — requires login) |
| `credentials.json` | Employee credentials (SHA-256 hashed passwords) |
| `add-employee.html` | **Admin tool** — use locally only, do NOT push to GitHub |

---

## Step 1 — Install Git

1. Go to **https://git-scm.com/download/win**
2. Download and run the installer (keep all defaults)
3. Open **Git Bash** from Start Menu to verify: type `git --version`

---

## Step 2 — Create a GitHub Repository

1. Go to **https://github.com** and sign in (or create a free account)
2. Click the **+** button → **New repository**
3. Name it: `aml-qc-engine` (or any name you prefer)
4. Set it to **Public** (required for free GitHub Pages)
5. Do NOT check "Add README" — click **Create repository**

---

## Step 3 — Push Your Files to GitHub

Open **Git Bash**, then run these commands one by one:

```bash
cd /c/Users/RajaRaman/Desktop/quality
git init
git add index.html aml_qc_scorer.html credentials.json
git commit -m "Initial deploy: AML QC Engine with login"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/aml-qc-engine.git
git push -u origin main
```

> Replace `YOUR_USERNAME` with your actual GitHub username.

---

## Step 4 — Enable GitHub Pages

1. On your GitHub repo page, go to **Settings → Pages**
2. Under **Source**, select **Deploy from a branch**
3. Branch: `main` — Folder: `/ (root)` — click **Save**
4. Wait ~60 seconds, then your site is live at:
   `https://YOUR_USERNAME.github.io/aml-qc-engine/`

---

## Managing Employee Access

### Default test credentials
| Employee ID | Password |
|-------------|----------|
| EMP001 | Slice@123 |
| EMP002 | Slice@456 |

### Adding a new employee

1. **Open `add-employee.html` locally** in your browser (double-click the file — do NOT use the GitHub Pages URL)
2. Enter the new Employee ID and password → click **Generate Entry**
3. Copy the output (e.g. `"EMP003": "abc123..."`)
4. Open `credentials.json` and add it inside the `{ }`:
   ```json
   {
     "EMP001": "5a66d81d...",
     "EMP002": "2b7699fd...",
     "EMP003": "abc123..."
   }
   ```
5. Push the updated file to GitHub:
   ```bash
   cd /c/Users/RajaRaman/Desktop/quality
   git add credentials.json
   git commit -m "Add employee EMP003"
   git push
   ```
   The change is live within ~30 seconds.

### Removing an employee

Delete their line from `credentials.json` and push. They will be locked out immediately.

### Resetting a password

Generate a new hash in `add-employee.html` and replace the old hash for that Employee ID in `credentials.json`, then push.

---

## Security Notes

- Passwords are stored as SHA-256 hashes — not plaintext
- The app uses `sessionStorage`, so each browser tab/session requires login
- The `credentials.json` file is publicly readable (it's a static site), but only hashes are stored — never raw passwords
- For stronger security in the future, consider adding Supabase or Netlify Identity as a backend
