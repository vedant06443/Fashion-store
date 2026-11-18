# Suyog Collection — cPanel Git Deployment & Verification Guide

This guide describes how to deploy updates to **Suyog Collection** (`https://suyogcollection.in`) on GoDaddy cPanel shared hosting (Apache).

---

## 1. Prerequisites (Set Once in `.cpanel.yml`)

Before the first deployment, ensure [`.cpanel.yml`](file:///./.cpanel.yml) has your exact cPanel credentials:
1. Log into your GoDaddy cPanel.
2. Navigate to **cPanel > Domains** and find the row for `suyogcollection.in`.
3. Check the **Document Root** column:
   - Primary domain: usually `/home/<username>/public_html/`
   - Addon domain: usually `/home/<username>/suyogcollection.in/` (or `/home/<username>/public_html/suyogcollection.in/`)
4. In [`.cpanel.yml`](file:///./.cpanel.yml), configure the `DEPLOYPATH` variable:
   ```yaml
   - export DEPLOYPATH=/home/YOUR_CPANEL_USER/YOUR_DOCUMENT_ROOT/
   ```

---

## 2. Deployment Steps

Whenever you push new updates to GitHub:

1. **Log in to cPanel**.
2. Under the **Files** section, click **Git™ Version Control**.
3. Locate the repository for `suyogcollection.in` and click **Manage**.
4. In the **Basic Information** tab:
   - Click **Update from Remote** (fetches the latest commit from GitHub).
   - Click **Deploy HEAD Commit** (executes `.cpanel.yml` tasks).

---

## 3. Verify Deployment Instantly

Open your browser and visit:
👉 **`https://suyogcollection.in/deploy-check.txt`**

It will output:
```text
commit <short_sha> deployed <UTC timestamp>
```

- If the commit SHA matches the latest commit on your GitHub `main` branch, your deployment succeeded and Apache is serving from the exact document root.
- If it shows a 404 or an old commit, verify that `DEPLOYPATH` in `.cpanel.yml` matches the Document Root shown under **cPanel > Domains**.

---

## 4. Live Storefront

Visit:
👉 **`https://suyogcollection.in/`**

- If your browser displays cached content, perform a hard refresh with `Ctrl + F5` (Windows) or `Cmd + Shift + R` (Mac).
- `public/.htaccess` automatically sends `Cache-Control: no-cache, must-revalidate` for all HTML files and `config.js` to ensure shoppers always see the newest version.
