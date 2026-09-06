# Unofficial Trench Wire RSS Feed

Generates an RSS feed for [The Trench Wire](https://www.trenchcrusade.com/trench-wire/)
by scraping the public listing page. Runs automatically once an hour via GitHub Actions
and publishes `feed.xml` through GitHub Pages.

## Setup (one-time)

1. **Create a new GitHub repository.**
   - Go to github.com → New repository (e.g. name it `trenchwire-feed`).
   - Public repo, no need to initialize with a README (you're adding these files).

2. **Add these files to the repo**, keeping this folder structure:
   ```
   /
   ├── scraper.py
   ├── requirements.txt
   ├── README.md
   └── .github/
       └── workflows/
           └── generate-feed.yml
   ```
   Note: the file `generate-feed.yml` needs to go inside a `.github/workflows/`
   folder — create that folder structure when uploading, or via `git` locally:
   ```
   mkdir -p .github/workflows
   mv generate-feed.yml .github/workflows/
   ```

3. **Enable GitHub Pages.**
   - In the repo, go to Settings → Pages.
   - Under "Build and deployment", set Source to **Deploy from a branch**.
   - Branch: `main`, folder: `/ (root)`.
   - Save.

4. **Run the workflow once manually** to generate the first `feed.xml`.
   - Go to the Actions tab → "Generate Trench Wire RSS Feed" → Run workflow.
   - This creates `feed.xml` in the repo and commits it.

5. **Your feed URL** will be:
   ```
   https://<your-username>.github.io/<repo-name>/feed.xml
   ```
   Paste that into any RSS reader.

## How it updates

The GitHub Action runs automatically every hour, re-scrapes the page, and commits
`feed.xml` if anything changed. GitHub Pages serves whatever is in the repo, so the
feed stays current without you doing anything further.

## If it breaks

If Trench Crusade changes the page's HTML structure, `parse_articles()` in
`scraper.py` may stop finding articles. Check the Actions tab for a failed run —
the script will exit with an error message if it finds zero articles, which is a
signal the site layout changed and the parsing logic needs updating.
