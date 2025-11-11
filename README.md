# Research Directory (GitHub Pages)


This is a minimal GitHub Pages site that reads a CSV in `_data/people.csv` and lets visitors filter by **Name**, **Institution**, and **Research area** — all client‑side, no backend.


## Quick start
1. **Create a new repository** on GitHub (public).
2. Add these files; put your data in `_data/people.csv` with headers:
- `First Name,Last Name,email address,institution,research area`
3. Commit and push.
4. In **Settings → Pages**, set:
- **Source:** `Deploy from a branch`
- **Branch:** `main` (or `master`) / `/ (root)`
5. Wait for Pages to build, then open the site.


## Updating the directory
Edit `_data/people.csv` (you can use GitHub’s web editor). Changes go live after the Pages build finishes.


## Customization
- Change site title/description in `_config.yml`.
- For custom styles, add CSS in `index.md`’s `<style>` block or create `assets/css/style.css` and include it.
- If your repository is a project site (i.e., `username.github.io/project`), set `baseurl: "/project"` in `_config.yml`.


## Notes
- Field searches are partial and case‑insensitive.
- Missing values render as an em‑dash.
- If you change CSV header names, update the Liquid keys in `index.md` to match.
