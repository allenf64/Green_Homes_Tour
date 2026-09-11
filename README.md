# Home Energy & Solar Performance Report — GitHub Pages site

This folder is a ready-to-publish, single-page site: a home page with the
full write-up, an embedded interactive dashboard (with a Solar Performance
tab and an Energy Bills & Savings tab), and a sources/authorship section —
all on one scrolling page. No coding required to get it live — just follow
the steps below.

## 1. Create a repository on GitHub

1. Go to [github.com/new](https://github.com/new).
2. Name it whatever you like — for example `solar-report`.
3. Set it to **Public** (GitHub Pages' free tier requires a public repo,
   unless you're on a paid plan).
4. Leave everything else unchecked, and click **Create repository**.

## 2. Upload these files

The easiest way, with no command line needed:

1. On your new repo's page, click **Add file → Upload files**.
2. Drag in every file from this folder (`index.md`, `dashboard.html`,
   `_config.yml`, and this `README.md`).
3. Scroll down and click **Commit changes**.

## 3. Turn on GitHub Pages

1. In your repo, click **Settings** (top menu bar).
2. In the left sidebar, click **Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**.
4. Under **Branch**, choose `main` and `/ (root)`, then click **Save**.
5. Wait about a minute, then refresh the page — GitHub will show you the
   live URL, something like:

   ```
   https://YOUR-USERNAME.github.io/solar-report/
   ```

That's it. That URL is now a public website anyone can visit.

## 4. Making changes later

Click any file in the repo, click the pencil icon (**Edit**), make your
change, and click **Commit changes**. The live site updates automatically
within a minute or two — no separate "publish" step.

## What each file does

| File | Purpose |
|---|---|
| `index.md` | The whole site — intro, full write-up for both the solar analysis and the energy-bill analysis, embedded dashboard, sources, and authorship, all on one scrolling page |
| `dashboard.html` | The interactive chart dashboard, embedded partway down the home page in an `<iframe>`. Has two tabs — Solar Performance and Energy Bills & Savings. Kept as raw HTML so the hover/interactive charts keep working (Markdown can't run the JavaScript charts need) |
| `_config.yml` | Tells GitHub Pages to use a built-in theme ("Cayman") so the site has a real look instead of plain unstyled text |

A note on the embed: `index.md` includes `dashboard.html` in an `<iframe>`
so the interactive charts show up directly in the scroll, with a link right
below it to open the dashboard full-page if the embedded box feels cramped
on a phone or small screen.

## Optional: use your own domain name

If you own a domain (e.g. from Namecheap or Google Domains), Settings →
Pages has a **Custom domain** field where you can point it at this site.
GitHub's own guide: <https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site>
