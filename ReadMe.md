# Emergent Metrics

A lightweight, no-subscription social media analytics workbook for the @emergent.sjsu internship. Interns plan content on a shared calendar, manually log per-post metrics and weekly account snapshots for Instagram, TikTok, YouTube, and LinkedIn, then review trend charts, platform comparisons, printable reports, and individual portfolio summaries. Everything runs in the browser as a single HTML file.

## Calendar and portfolios

The Calendar tab holds planned posts with a date, label, format, owner, and a status that moves from idea through drafted, filmed, and approved to posted. Entries appear on a month grid color-coded by platform, and clicking one opens it for editing. When a post is marked posted, a "Log metrics" button carries its details straight into the Log data form so the intern only adds the numbers.

The Portfolio tab generates a one-page performance summary for any intern, built from posts logged with the "Posted by" field. It includes their totals, average engagement rate, strongest post, and a full post table, formatted for printing so students can save it as a PDF for job applications. If you already deployed the Google Sheet backend before these features existed, paste the updated `google-apps-script.gs` over the old script, then use Deploy, Manage deployments, and edit to a new version; the script will create the new "calendar" tab on first use.

## Deploying to GitHub Pages

1. Create a new repository on GitHub (public or private with Pages enabled).
2. Upload `index.html` to the repository root.
3. In the repository, open Settings, then Pages, and set the source to "Deploy from a branch" with the `main` branch and `/ (root)` folder.
4. The site will be live within a minute or two at `https://YOURUSERNAME.github.io/REPONAME/`.

## Team sync with a shared Google Sheet

By default the site stores data only in each browser. Connecting a Google Sheet gives the whole team one shared dataset that survives cleared browsers and graduating interns. Setup takes about twenty minutes once.

1. Create a new Google Sheet from the account that should own the data (a program account works well). Name it something like "Emergent Metrics Data".
2. In the Sheet, open Extensions and choose Apps Script. Delete any starter code and paste in the entire contents of `google-apps-script.gs` from this repository. Save the project.
3. Click Deploy, then New deployment. Choose type "Web app". Set "Execute as" to Me, and "Who has access" to Anyone. Click Deploy and authorize when Google prompts you (you may need to click through an "unsafe app" warning, which appears because the script is your own unverified code).
4. Copy the web app URL, which ends in `/exec`.
5. Open the site, click the sync status button in the header (it reads "Local only"), paste the URL, and click "Connect & sync". The status changes to "Synced" and any data already in that browser uploads to the Sheet.
6. Share the same URL with each intern so they can connect their browsers. Any data they logged locally merges into the Sheet on first sync.

Two things to know. First, anyone who has the URL can write to the Sheet, so treat the link like a shared password and share it only with the team; if it ever leaks, create a new deployment to get a fresh URL. Second, the "posts" and "snapshots" tabs in the Sheet are safe to view, sort, and chart from, but avoid editing the id column or reordering columns, since the site matches rows by id. If you update the script later, use Deploy, then Manage deployments, and edit the existing deployment to a new version so the URL stays the same.

## How data is stored

Data always saves to the browser's localStorage first, so the site works offline and entries are never lost to a network hiccup. When team sync is connected, each entry also writes to the shared Sheet, and on page load the site merges the Sheet's data with anything logged locally. Without sync, data stays on that one machine. The Report tab includes a "Download backup (JSON)" button for moving data between machines or archiving at the end of a semester, and "Restore backup" to load it elsewhere. CSV exports of posts and snapshots are available for analysis in Excel or Sheets.

## Engagement rate formula

Per-post engagement rate is calculated as (likes + comments + shares + saves) ÷ reach × 100. If reach is blank, views are used as the denominator instead. The Log data form shows this calculation live as numbers are entered.

## Adding platforms or metrics

Platforms live in one config object at the top of the script in `index.html` (`const PLATFORMS`). Adding a platform is one new line with a name and color. New metric fields require adding an input to the form and a column to the tables, which is a nice stretch task for an intern learning web development.
