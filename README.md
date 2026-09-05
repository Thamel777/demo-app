# Git & GitHub — A Practical Command Guide

**Project:** `demo-app` · **Goal:** understand the *why* behind each command, not just memorize it.

---

## Table of contents

1. [Core concepts in one minute](#1-core-concepts-in-one-minute)
2. [The vocabulary you'll hear daily](#2-the-vocabulary-youll-hear-daily)
3. [Branching strategy](#3-branching-strategy)
4. [One-time setup](#4-one-time-setup)
5. [The everyday workflow](#5-the-everyday-workflow)
6. [Commit messages that don't suck](#6-commit-messages-that-dont-suck)
7. [Problems & fixes](#7-problems--fixes)
8. [Command cheat sheet](#8-command-cheat-sheet)

---

## 1. Core concepts in one minute

**Git** is a "super-smart Save button" that lives on *your computer*. Each save point is a **commit** — a full snapshot of the whole project that you can roll back to anytime.

**GitHub** is the online cloud backup the whole team shares. If Git is your local save button, GitHub holds the official master copy, and it's where teammates review each other's code.

| | Git | GitHub |
|---|---|---|
| Where it lives | Your machine (local) | The cloud (remote) |
| What it's for | Saving snapshots locally | Sharing & reviewing with the team |
| Nickname | "the save button" | "origin" |

A **repository** (repo) is just a fancy word for a project's folder. One project = one repo. Ours is `demo-app`.

---

## 2. The vocabulary you'll hear daily

| Term | Plain meaning |
|---|---|
| **Commit** | A save point — a snapshot of your code. |
| **Clone** | Your first-ever download of a repo. |
| **Push** | Upload your local commits to GitHub. |
| **Pull** | Download the latest commits from GitHub. |
| **Branch** | A safe copy of the project where you work without touching the master copy. |
| **Merge** | Combine commits from one branch into another. |
| **Pull Request (PR)** | A formal request to merge — where review happens. |
| **Origin** | The remote repo on GitHub (e.g. `origin/dev`). |
| **Conflict** | Two people edited the same line — needs a human to decide. |

---

## 3. Branching strategy

Think of `main` as a tree trunk. Your feature branch grows off it, then merges back when it's done. Your branch never affects the trunk, so `main` stays stable the whole time.

### The two branches we never touch directly

| Branch | Purpose | Analogy | Your access |
|---|---|---|---|
| **`main`** *(a.k.a. master)* | The 100% stable code customers use right now. | The final, published book on sale in stores. | Off-limits — only senior leads manage it. |
| **`dev`** *(a.k.a. develop)* | The integration branch — features are merged & tested together here. | The "final draft" where all chapters come together. | **Your starting point for every task.** |

### Local vs. remote — the "source of truth"

- **`dev`** on your computer is just a *copy*. It can fall out of date the moment a teammate merges something.
- **`origin/dev`** on GitHub is **the single source of truth** — the official version the whole team trusts.

> **Golden habit:** before starting any new work, run `git pull origin dev` to match the source of truth.

### The 5 golden rules of branching

Follow these and you'll avoid 90% of common problems.

1. **Never commit to `main` or `dev`.** Your work only ever happens on `feature/` or `fix/` branches.
2. **Always start from an up-to-date `dev`.** `git checkout dev` → `git pull origin dev` → *then* branch.
3. **Name your branch clearly.** `feature/add-contact-form` beats a vague `feature/mybranch`.
4. **Keep branches small & focused.** One task = one branch. Don't bundle unrelated work.
5. **Sync early, sync often.** Merge `dev` in daily to fix small conflicts, not one scary one.

**Branch naming prefixes:** `feature/` for new work · `fix/` for bugs · `hotfix/` for emergencies.

---

## 4. One-time setup

Do this once per machine.

```bash
# Tell Git who you are (shows up on every commit)
git config --global user.name  "Your Name"
git config --global user.email "you@example.com"
```

Then get access and download the project **once**:

1. Share your GitHub username with the lead → accept the collaborator invite email.
2. Clone the repo — this downloads a full copy of `demo-app` to your machine:

```bash
git clone https://github.com/your-org/demo-app.git
cd demo-app
```

---

## 5. The everyday workflow

This is the loop you repeat for *every* task:

**Branch → Commit → Push → Pull Request → Merge → Clean up**

### Step 1 — Start every task from a fresh `dev`

```bash
# switch to dev and get the latest from the source of truth
git checkout dev
git pull origin dev

# create your safe feature branch off the up-to-date dev
git checkout -b feature/add-contact-form

# you are now safe to start working
```

### Step 2 — The commit loop (save checkpoints often)

As you code, save small snapshots:

```bash
git add .                              # stage your changed files
git commit -m "feat: add contact form" # save the snapshot
```

Repeat `add` + `commit` as often as you like — they're your safety net. (See [good messages](#6-commit-messages-that-dont-suck) below.)

> Tip: `git status` shows what's changed, and `git diff` shows the exact lines. Run them anytime you're unsure.

### Step 3 — Push your branch to GitHub

```bash
# -u only on the FIRST push of a new branch — it links local ↔ remote
git push -u origin feature/add-contact-form
```

After that first push, every later push on the same branch is just:

```bash
git push
```

### Step 4 — Open a Pull Request

A Pull Request is you asking the team: *"please review my work and merge it into `dev`."* Open it on GitHub from your pushed branch.

A great PR description answers:

- **What** — "Adds a contact form to the `/contact` page."
- **Why** — "For Task #23 — lets users send us messages."
- **How to test** — "Go to `/contact`, submit, check it saved."
- **Anything out of plan** — flag extras like new env vars, config, or helpers.

### Step 5 — Review, merge & clean up

- **If changes are requested:** edit on the *same* branch, then `add` + `commit` + `push` again. The PR updates automatically — no new PR needed.
- **If approved:** a senior dev with merge permissions clicks **Merge**. Your work is now officially in `dev`.

Then clean up your finished branch:

```bash
git checkout dev                       # back to dev
git pull origin dev                    # grab your just-merged work
git branch -d feature/add-contact-form # delete the local branch
```

You're ready for the next task — back to Step 1.

---

## 6. Commit messages that don't suck

Start every message with a **type** (Conventional Commits):

| Type | Use for |
|---|---|
| `feat:` | a new feature |
| `fix:` | a bug fix |
| `docs:` | documentation changes |
| `style:` | formatting / CSS only, no logic change |
| `refactor:` | restructure, same behaviour |

```bash
git commit -m "feat: add contact form to /contact page"
git commit -m "fix: correct nav spacing on mobile"
git commit -m "docs: update demo-app setup steps"
```

---

## 7. Problems & fixes

Don't panic — almost every Git error is recoverable. Read the message, sync with `dev`, and ask your lead when in doubt.

### "Your local changes would be overwritten"

You edited files but haven't committed, and `pull` wants to touch the same files. Tuck your changes aside, pull, then bring them back:

```bash
git stash      # hide your uncommitted changes
git pull       # now safe to pull
git stash pop  # bring your changes back
```

### "My branch is old — dev moved on"

Not an error yet, but it *will* cause conflicts later. Sync now to handle small conflicts on your own terms:

```bash
git checkout dev
git pull origin dev
git checkout feature/add-contact-form
git merge dev
```

### Merge conflicts — the big one

**This is normal!** Two people edited the same line, so Git can't choose — it needs a human. You'll see markers like this in the file:

```text
<<<<<<< HEAD
const color = "blue";   // YOURS
=======
const color = "red";    // from dev
>>>>>>> dev
```

Git is literally asking: *"which one is correct — blue, red, or something else?"*

**How to fix it:**

1. Don't panic. Open the file(s) Git lists.
2. Delete the markers `<<<<<<<`, `=======`, `>>>>>>>`.
3. Edit the text so it's correct.
4. Save the file.
5. Stage & commit the resolution:

```bash
git add .
git commit          # completes the merge
```

### Push rejected (non-fast-forward)

GitHub has commits you don't. Pull first to merge them, then push:

```bash
git pull
git push
```

### "Merge" button is greyed out on GitHub

This is **not** a Git error — it's a branch-protection rule. A quality check hasn't passed yet. Common causes:

- **Failed checks** — a test is red. Open **Details**, fix, commit & push.
- **Not enough approvals** — you may need a second reviewer to approve.
- **Branch out of date** — sync `dev` into your branch (see above), then push again.

---

## 8. Command cheat sheet

```bash
# --- setup (once) ---
git clone <repo-url>            # download demo-app the first time

# --- start a task ---
git checkout dev               # go to dev
git pull origin dev            # get the source of truth
git checkout -b feature/<name> # make your branch

# --- while working ---
git status                     # what's changed?
git diff                       # exact lines changed
git add .                      # stage changes
git commit -m "feat: ..."      # save a snapshot

# --- share & review ---
git push -u origin feature/<name>  # first push
git push                           # every push after
# ...then open a Pull Request on GitHub

# --- clean up after merge ---
git checkout dev
git pull origin dev
git branch -d feature/<name>

# --- rescue moves ---
git stash / git stash pop      # tuck changes aside / bring them back
git merge dev                  # sync dev into your branch
git pull && git push           # fix a rejected push
```

---

### Your everyday loop, in one line

> **One task = one branch.** Always branch off an up-to-date `dev`, commit small with clear messages, sync often, and let PRs keep the whole team's code stable and clean.
