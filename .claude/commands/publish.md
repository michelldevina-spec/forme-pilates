---
description: Security-scan the repo, refresh README, commit & push to GitHub, and verify GitHub Pages (Actions) + repo "About" metadata
---

Run the full publish workflow for this repo (FORME Pilates landing page → GitHub Pages at `michelldevina-spec/forme-pilates`). Work through the steps below **in order**. If any step in 1–2 finds a problem, **stop and report it to the user before pushing anything** — do not proceed to step 4 until it's resolved.

## 1. Security scan (gate — must pass before pushing)

Scan the working tree and full git history for secrets/credentials before anything is committed or pushed:

- Search tracked + new files for common secret patterns: AWS keys (`AKIA...`), GitHub tokens (`ghp_`, `gho_`, `github_pat_`), OpenAI-style keys (`sk-...`), Slack tokens (`xox...`), Google API keys (`AIza...`), private key blocks (`-----BEGIN ... PRIVATE KEY-----`), and `password=`/`secret=`/`api_key=` style assignments with literal values.
- Check for accidentally-added `.env`, `*.pem`, `*.key`, or `*credentials*` files (`.gitignore` already excludes these — confirm nothing matching is staged anyway with `git status --porcelain`).
- If this is not the first publish, also scan `git log -p` for the same patterns in case a secret was committed and later "removed" (removal alone doesn't scrub history).
- The FormSubmit recipient address in `#bookingForm`'s `action` attribute (`michelldevina@gmail.com`) is intentional/public per `CLAUDE.md` — not a finding.

If anything suspicious is found: **stop**, report the file/line to the user, and do not push until it's removed (and rotated/purged from history if already committed).

## 2. README

Verify `README.md` exists at the repo root and is reasonably up to date with the current page sections, deployment setup, and placeholder-contact-details list described in `CLAUDE.md`. Update it if the page structure has materially changed since it was last written. Don't rewrite it from scratch if it's already accurate.

## 3. Repo metadata reminder

Repo "About" (description/homepage/topics) and the **Settings → Pages → Source = GitHub Actions** toggle live in GitHub's repo settings, not in this codebase. Check whether the `gh` CLI is available and authenticated (`gh auth status`):

- **If `gh` is available and authenticated**, run:
  ```
  gh repo edit michelldevina-spec/forme-pilates \
    --description "Boutique reformer Pilates studio in Singapore — marketing landing page" \
    --homepage "https://michelldevina-spec.github.io/forme-pilates/" \
    --add-topic pilates --add-topic landing-page --add-topic singapore --add-topic github-pages
  ```
  and confirm the Pages build type is set to GitHub Actions:
  ```
  gh api -X PUT repos/michelldevina-spec/forme-pilates/pages -f build_type=workflow
  ```
- **If `gh` is not available**, skip this step silently on routine runs — don't re-prompt the user every time. Only mention it once if the repo description/homepage still look unset (`description: null` via `curl -s https://api.github.com/repos/michelldevina-spec/forme-pilates`).

## 4. Commit & push

- `git status` / `git diff` to review what changed.
- Stage the relevant files (avoid `git add -A` if there's anything unexpected in `git status`).
- Commit with a concise message describing the change.
- Push to `origin main`.

## 5. Verify GitHub Pages deployment

- `.github/workflows/deploy.yml` runs on every push to `main` and deploys the repo root via `actions/upload-pages-artifact` + `actions/deploy-pages`. Confirm this workflow file is present and unchanged unless intentionally modified.
- If `gh` is available, check the latest run: `gh run list --workflow=deploy.yml --limit 1` and `gh run view --log-failed` if it failed.
- Otherwise, just note that the push will trigger the deploy workflow and the live site is at https://michelldevina-spec.github.io/forme-pilates/.

## 6. Final report

Summarize: what was scanned, what changed, what was pushed, and the live Pages URL. Flag anything still needing manual action (e.g., placeholder contact details in `CLAUDE.md`, or repo "About"/Pages settings if `gh` wasn't available).
