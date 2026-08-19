# github-basics

Two purposes for this repo:

1. **Learn git/GitHub by doing** — see the "Try it yourself" section below.
2. **Keep this GitHub account showing activity** — an automated workflow
   commits a small update here every ~4 weeks, indefinitely, without any
   manual work from me. See [How the automation works](#how-the-automation-works).

## Repo layout

| File | Purpose |
|---|---|
| `README.md` | This file. |
| `PLAYGROUND.md` | A scratch file for you to edit by hand as practice. |
| `CHANGELOG.md` | Log of updates — both manual and automated. |
| `.github/workflows/keep-active.yml` | The automation (see below). |
| `.github/last_update.txt` | Marker file the automation uses to track cadence. |

## Try it yourself — the basic git/GitHub loop

All of this from a terminal that already has `git` and `gh` set up
(this machine already does):

```bash
# 1. Make sure you're up to date
cd ~/github-basics
git pull

# 2. Make a branch for your change (never edit master directly — good habit)
git checkout -b practice/edit-playground

# 3. Edit a file
#    e.g. open PLAYGROUND.md and add a line under "Notes"

# 4. Stage and commit
git add PLAYGROUND.md
git commit -m "Add a note to PLAYGROUND"

# 5. Push your branch to GitHub
git push -u origin practice/edit-playground

# 6. Open a pull request (PR) from your branch into master
gh pr create --fill

# 7. Merge it (on GitHub.com, or from the terminal)
gh pr merge --merge

# 8. Clean up
git checkout master
git pull
git branch -d practice/edit-playground
```

That loop — **branch → edit → commit → push → PR → merge** — is basically
all of GitHub. Everything else (issues, reviews, Actions, etc.) builds on
top of it.

Other useful commands to try:

```bash
git log --oneline          # see commit history
git diff                   # see uncommitted changes
gh issue create            # open an issue on this repo
gh repo view --web         # open the repo in a browser
```

## How the automation works

`.github/workflows/keep-active.yml` runs on GitHub's servers (not on my
machine) every Monday. Each run:

1. Reads `.github/last_update.txt` for the date of the last automated update.
2. If fewer than 28 days have passed, it does nothing (no-op).
3. If 28+ days have passed, it appends a dated line to `CHANGELOG.md`,
   updates `.github/last_update.txt` to today, and commits/pushes as
   `github-actions[bot]`.

Net effect: roughly one small, real, dated commit every 4 weeks, forever,
with zero maintenance — while manual practice commits (from the loop above)
can happen any time on top of it.

You can also trigger it manually to test it:

```bash
gh workflow run keep-active.yml
gh run watch
```
