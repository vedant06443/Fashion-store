# Suyog Collection — cPanel Git Deployment & Verification Guide

This guide describes how to deploy updates to **Suyog Collection** (`https://suyogcollection.com`) on GoDaddy cPanel shared hosting (Apache).

---

## 1. Prerequisites (Set Once in `.cpanel.yml`)

Before the first deployment, confirm the document root that [`.cpanel.yml`](file:///./.cpanel.yml) deploys to:
1. Log into your GoDaddy cPanel.
2. Navigate to **cPanel > Domains** and find the row for `suyogcollection.com`.
3. Check the **Document Root** column:
   - Primary domain: usually `/home/<username>/public_html/`
   - Addon domain: usually `/home/<username>/suyogcollection.com/` (or `/home/<username>/public_html/suyogcollection.com/`)
4. `.cpanel.yml` uses the literal document-root path on every task (cPanel runs each task in its own shell, so a
   `DEPLOYPATH` variable would not carry over). If the Document Root ever changes, find-and-replace
   `/home/c9fct449atzo/public_html/` with the new path throughout `.cpanel.yml`.

---

## 2. Deployment Steps

Whenever you push new updates to GitHub:

1. **Log in to cPanel**.
2. Under the **Files** section, click **Git™ Version Control**.
3. Locate the repository for `suyogcollection.com` and click **Manage**.
4. In the **Basic Information** tab:
   - Click **Update from Remote** (fetches the latest commit from GitHub).
   - Click **Deploy HEAD Commit** (executes `.cpanel.yml` tasks).

---

## 3. Verify Deployment Instantly

Open your browser and visit:
👉 **`https://suyogcollection.com/deploy-check.txt`**

It will output:
```text
commit <short_sha> deployed <UTC timestamp>
```

- If the commit SHA matches the latest commit on your GitHub `main` branch, your deployment succeeded and Apache is serving from the exact document root.
- If it shows a 404 or an old commit, verify that the paths in `.cpanel.yml` match the Document Root shown under **cPanel > Domains**.

---

## 4. Live Storefront

Visit:
👉 **`https://suyogcollection.com/`**

- If your browser displays cached content, perform a hard refresh with `Ctrl + F5` (Windows) or `Cmd + Shift + R` (Mac).
- `public/.htaccess` automatically sends `Cache-Control: no-cache, must-revalidate` for all HTML files and `config.js` to ensure shoppers always see the newest version.
