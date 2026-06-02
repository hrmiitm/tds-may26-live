# Day 5 Teaching Guide — Git & GitHub Workflow
## TDS Bridge Bootcamp

> **Who this is for:** Students who have been creating files for 4 days and pushing them informally. Today they learn Git properly — the mental model, the daily commands, authentication, and how to ship a version.
> **Goal by end of session:** Student can confidently run the daily Git workflow, set up SSH auth, and tag + release a version on GitHub.

---

## Section 1 — The Git Mental Model (15 min)

### Why Git feels confusing at first

Most beginners treat Git like a backup system: "I copy my files to GitHub." That's not what Git is. Git is a **timeline of your project** — every commit is a snapshot of your entire project at a point in time. You can travel back to any snapshot.

The confusion comes because Git has three places your changes can live before they're part of the timeline.

### The three states

```
Your files on disk         Staging area            Git history
(Working Tree)             (Index)                 (Commits)
─────────────────          ─────────────           ──────────────────
You edit files here   →   You choose what    →    Permanent snapshots.
                          goes into the            Each has a message,
                          next snapshot.           author, and timestamp.
```

**The key insight:** Editing a file does *not* save it to Git. You have to explicitly say "include this file in the next snapshot" (`git add`) and then "take the snapshot" (`git commit`).

This feels annoying at first. It's actually powerful — you can edit 10 files but only commit 3 of them if the other 7 aren't ready.

### Visualising it

```
─── Working Tree ──────────────────────────────────
   app.py (modified)
   README.md (modified)
   notes.txt (untouched)

─── Staging Area (after git add app.py) ───────────
   app.py ✓  ← will be in the next commit
   README.md    ← not staged, won't be included

─── Committed (after git commit) ──────────────────
   Snapshot: "fix: handle empty response from API"
   Contains only app.py
   README.md changes are still in working tree
```

---

**Section summary:** Git tracks snapshots, not file copies. Changes go: Working Tree → Staging Area (`git add`) → Commit (`git commit`). Only staged files end up in a commit.

---

## Section 2 — The Daily Commands (20 min)

### The three commands you run constantly

These three tell you the state of your repo at any moment. Run them before and after every action until they become habit.

```bash
git status    # What files are modified, staged, or untracked?
git diff      # What exact lines changed in modified files?
git log       # What commits exist? Who made them? When?
```

`git log --oneline` gives a compact, readable history:

```
a3f9c12 fix: handle empty API response
b21d4e8 feat: add POST /echo endpoint
c10a991 chore: initial FastAPI setup
```

### The core workflow — every single day

```bash
# 1. See what changed
git status

# 2. Review the changes
git diff

# 3. Stage what you want in the commit
git add app.py              # Stage one file
git add .                   # Stage everything (use carefully)
git add -p                  # Interactive — stage chunks, not whole files

# 4. Commit with a meaningful message
git commit -m "feat: add timestamp to echo response"

# 5. Push to GitHub
git push
```

### Writing good commit messages

A commit message should complete the sentence: *"If applied, this commit will ___"*

| Bad | Good |
|-----|------|
| `"stuff"` | `"fix: handle empty API response"` |
| `"final"` | `"feat: add POST /echo endpoint"` |
| `"changes"` | `"docs: add curl examples to README"` |
| `"update"` | `"chore: add .gitignore for Python projects"` |

You don't need to follow a formal convention — just write one clear sentence that explains *what* and *why*.

### Undoing things safely

```bash
# Discard changes in a file (before staging)
git restore app.py

# Unstage a file (after git add, before git commit)
git restore --staged app.py

# Undo the last commit but keep the changes in working tree
git reset --soft HEAD~1

# See what a commit changed
git show a3f9c12
```

> ⚠️ Never use `git reset --hard` unless you're sure you want to permanently discard changes. Start with `--soft`.

---

**Section summary:** `git status`, `git diff`, `git log` are your daily compass. The workflow is always: status → diff → add → commit → push. Write commit messages as one clear sentence. Use `git restore` to undo safely.

---

## Section 3 — Remotes: origin and main (10 min)

### What is a remote?

A **remote** is another copy of your repository stored somewhere else — on GitHub, in this case. When you push, your local commits get sent there. When you pull, you get commits from there.

```bash
git remote -v
```

Output:
```
origin  git@github.com:yourname/tds-bootcamp.git (fetch)
origin  git@github.com:yourname/tds-bootcamp.git (push)
```

`origin` is the default name for your GitHub remote. It's just a nickname for the URL — nothing special about the word "origin".

### What is main?

`main` is the default branch — think of it as the primary timeline. Branches let you have parallel timelines (a feature branch, a hotfix branch) but for now, you're always on `main`.

```bash
git branch          # See which branch you're on
git push            # Short for: git push origin main
git push -u origin main   # First push — sets the default remote/branch
```

### Linking a local repo to GitHub

If you created a repo locally (not cloned from GitHub):

```bash
git init                                         # Initialise Git
git add .                                        # Stage everything
git commit -m "chore: initial commit"            # First commit
git remote add origin git@github.com/yourname/reponame.git  # Link to GitHub
git push -u origin main                          # Push and set default
```

---

**Section summary:** `origin` is the nickname for your GitHub remote. `main` is the default branch. `git push` sends commits to GitHub. `git pull` gets new commits from GitHub.

---

## Section 4 — Authentication: HTTPS vs SSH (20 min)

### Why you can't just use your password

GitHub stopped accepting passwords for Git operations in 2021. You now need either:
- A **Personal Access Token (PAT)** — a long generated password you paste
- An **SSH key pair** — a cryptographic key stored on your machine

### Option A: SSH (recommended — set it once, never enter credentials again)

SSH uses a **key pair**: a private key that stays on your machine, and a public key you give to GitHub. When you push, your machine proves it has the private key without revealing it.

```bash
# Step 1: Generate an SSH key (run once, ever)
ssh-keygen -t ed25519 -C "youremail@example.com"
# Press Enter to accept the default location (~/.ssh/id_ed25519)
# Optionally set a passphrase (adds security)

# Step 2: Copy your public key
cat ~/.ssh/id_ed25519.pub
# Copy the entire output

# Step 3: Add to GitHub
# → github.com → Settings → SSH and GPG keys → New SSH key
# → Paste the public key, give it a name like "My Laptop"

# Step 4: Test the connection
ssh -T git@github.com
# Expected: "Hi yourname! You've successfully authenticated..."
```

Now set your remote to use SSH (if it's currently HTTPS):

```bash
git remote set-url origin git@github.com:yourname/reponame.git
```

From now on, `git push` works without any password prompt.

### Option B: HTTPS with a Personal Access Token (PAT)

```bash
# Your remote URL looks like:
# https://github.com/yourname/reponame.git

# Generate a PAT on GitHub:
# → Settings → Developer settings → Personal access tokens → Tokens (classic)
# → Generate new token → tick "repo" scope → copy the token

# When git asks for your password, paste the PAT (not your GitHub password)
```

> Use a credential manager (`git config --global credential.helper store`) so you don't have to paste the PAT every time.

### Understanding the difference conceptually

| | HTTPS + PAT | SSH |
|--|-------------|-----|
| URL format | `https://github.com/...` | `git@github.com:...` |
| Credential | Long token string | Private key file |
| Setup | Generate token on GitHub website | Generate key on your machine |
| Daily use | May prompt for token (if not cached) | Never prompts after setup |
| Best for | Quick one-off setup | Daily use on your machine |

### How this relates to API auth

The same concepts show up when calling APIs:

| Git SSH | API equivalent |
|---------|----------------|
| Private key on your machine | API secret key in `.bashrc` |
| Public key on GitHub | API key registered in their dashboard |
| Proves identity without sending a password | Bearer token in `Authorization` header |

The underlying idea — "prove you own a secret without exposing it" — is the same.

---

**Section summary:** Password auth is gone. Use SSH (recommended) or HTTPS + PAT. SSH setup: generate key pair → add public key to GitHub → test with `ssh -T`. After setup, `git push` never asks for credentials again.

---

## Section 5 — Tags and Releases (10 min)

### What is a tag?

A tag is a label on a specific commit — like a bookmark in your history. Tags are used for releases: "this is what v1.0.0 looked like."

Two types:
- **Lightweight tag:** Just a pointer to a commit. Like a bookmark.
- **Annotated tag:** Has a message, author, and date. Use this for releases.

```bash
# Create an annotated tag
git tag -a v0.1.0 -m "First working version — TDS Bridge Bootcamp complete"

# Push the tag to GitHub
git push origin v0.1.0

# See all tags
git tag

# See what a tag points to
git show v0.1.0
```

### Semantic versioning basics

Version numbers follow the pattern `MAJOR.MINOR.PATCH`:

| Part | When to increment |
|------|-------------------|
| `MAJOR` | Breaking change — old code won't work |
| `MINOR` | New feature — old code still works |
| `PATCH` | Bug fix — behaviour unchanged |

- `v0.1.0` — early development, not production ready
- `v1.0.0` — first stable release
- `v1.1.0` — added a feature
- `v1.1.1` — fixed a bug in that feature

### GitHub Releases

A GitHub Release is a page on your repo that packages a tag with release notes, download links, and optionally attached files. It makes your work discoverable and shareable.

**Create via GitHub UI:**
1. Go to your repo → Releases → **Create a new release**
2. Choose tag `v0.1.0` (or create it here)
3. Write a title and description: "TDS Bridge Bootcamp Complete"
4. Click **Publish release**

---

**Section summary:** Tags label specific commits. Always use annotated tags (`-a`) for releases. Semantic versioning: `MAJOR.MINOR.PATCH`. GitHub Releases wrap a tag with notes and make it public.

---

## Live Exercise — Do Together (15 min)

```bash
# 1. Check the state of your repo
cd ~/tds/bootcamp/day4-api    # or wherever your FastAPI project is
git status
git log --oneline

# 2. Make a small change
echo "# TDS Bridge Bootcamp — Day 4 API" >> README.md
git status
git diff

# 3. Stage and commit
git add README.md
git commit -m "docs: add project description to README"

# 4. Make a second commit
echo '{"note": "committed from terminal"}' > notes.json
git add notes.json
git commit -m "chore: add notes file"

# 5. Check the history
git log --oneline

# 6. Push
git push

# 7. Tag this version
git tag -a v0.1.0 -m "Bridge bootcamp complete"
git push origin v0.1.0

# 8. Go to GitHub and create a Release from the tag
```

**Discuss:** Open `git log --oneline` on the projector. What do the commit hashes represent? What happens if you run `git show <hash>`?

---

**End of Day 5 — and the Bridge Bootcamp**

> Remind students: complete the checklist, push `day5.md`, create the release, and share the repo URL. They are now ready to start TDS.
