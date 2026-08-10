# Science Olympiad Event Picker — Setup Guide

A free website your team can use to pick Science Olympiad events, add teammates,
and submit their choices — all collected in a Google Sheet you own. It runs on
GitHub Pages (free) and stores data in Google Sheets (free). No coding required
to set it up.

This guide takes you from zero to a working site in about 20 minutes.

---

## What you'll end up with

- A public web page (e.g. `https://yourname.github.io/scioly/`) students open.
- Each student enters their name, picks events, adds teammates, and submits once.
- A Google Sheet that fills in automatically: one tab with every submission, and
  one tab that merges teams by event.

## What you need

- A **GitHub account** (free) — https://github.com/signup
- A **Google account** (free). A **personal Gmail** works best; school accounts
  often block the public access this needs (see Troubleshooting).
- The project files: `index.html`, `Code.gs`, and `.github/workflows/deploy.yml`.

---

## Step 1 — Put the code on GitHub

1. Log in to GitHub and click **New** to create a repository.
2. Name it something like `scioly` and choose **Public** (required for free
   GitHub Pages). Check **Add a README**. Click **Create repository**.
3. Click **Add file -> Upload files**, then drag in `index.html`.
4. To add the deploy file, click **Add file -> Create new file** and type this
   exact name (the slashes create the folders):
   ```
   .github/workflows/deploy.yml
   ```
   Paste in the contents of `deploy.yml`, then **Commit changes**.

## Step 2 — Turn on the website (GitHub Pages)

1. In your repo, go to **Settings -> Pages**.
2. Under **Build and deployment -> Source**, choose **GitHub Actions**.
3. Wait about a minute, then reload the Pages settings page — your live web
   address appears at the top. That's your site (it won't collect data yet).

## Step 3 — Create the Google Sheet that collects data

1. Go to https://sheets.google.com and create a **blank spreadsheet**.
2. Click **Extensions -> Apps Script**.
3. Delete anything in the editor, paste in the entire `Code.gs` file, and click
   **Save**.

## Step 4 — Publish the Sheet's collector

1. In Apps Script, click **Deploy -> New deployment**.
2. Click the gear -> **Web app**. Set:
   - **Execute as:** Me
   - **Who has access:** Anyone
3. Click **Deploy**. Approve the permission prompt (choose your account ->
   **Advanced -> Go to (project) (unsafe) -> Allow** — it's your own script).
4. Copy the **Web app URL** it gives you. It ends in `/exec`.

## Step 5 — Connect the site to your Sheet

1. Back in GitHub, open `index.html` and click the **pencil** to edit.
2. Near the top of the `<script>` section, find:
   ```js
   const SUBMIT_URL = "PASTE_YOUR_APPS_SCRIPT_WEB_APP_URL_HERE";
   ```
3. Replace the placeholder with your `/exec` URL (keep the quotes).
4. **Commit changes.** GitHub redeploys the site in about a minute.

## Step 6 — Test it

1. Open your `/exec` URL in a browser with `?callback=test` on the end:
   ```
   https://script.google.com/…/exec?callback=test
   ```
   You should see `test({"status":"alive", ...})`. If you see a Google sign-in
   page, your access isn't set to **Anyone** (see Troubleshooting).
2. Open your live site, submit a test entry, and check the Sheet — a
   **Submissions** tab appears with the data.

You're done. Share your site's web address with your team.

---

## Where your data shows up

- **Submissions tab** — one row per student: their name and the events/teammates
  they chose.
- **Teams by Event tab** — rebuilt automatically: each event with its team, where
  identical teams (the same people) merge into one row. The last column shows how
  many of those people submitted it, so you can spot unconfirmed pairings.

Each student can submit only **once** — a repeat from the same name is rejected.

## Reset for a new season

Delete the rows in the **Submissions** tab (keep the header), or delete the tab
entirely — it rebuilds on the next submission. Everyone can then submit fresh.

## Change the events or team sizes

In `index.html`, the `EVENTS` list near the top of the `<script>` holds each
event. Every event has a `max` (team-size limit) you can edit, plus its name,
discipline, and description. Edit, commit, and it redeploys.

---

## Troubleshooting

- **Friends or Safari can't submit / it does nothing.** Your Apps Script isn't
  truly public. Make sure **Who has access** is **Anyone**, and that the URL in
  `index.html` ends in **/exec** (not `/dev`).
- **School Google account.** School accounts often don't offer real "Anyone"
  access. Fix: build the Sheet + Apps Script under a **personal Gmail**, then
  share the Sheet to your school email so you still see results.
- **You changed `Code.gs` but nothing updated.** Editing isn't enough — redeploy:
  **Deploy -> Manage deployments -> pencil/edit -> Version: New version -> Deploy.**
- **Site didn't update after a commit.** Check the **Actions** tab for a green
  check, then hard-refresh your browser (Ctrl/Cmd+Shift+R).

---

## Credits

President Kaaviyan Kannaiyan for the Science Olympiad Division C season 2026-2027. Not
affiliated with Science Olympiad, Inc. Event names, categories, and rules belong
to Science Olympiad, Inc. (https://www.soinc.org) — always confirm details in the
official rule book for each event.
