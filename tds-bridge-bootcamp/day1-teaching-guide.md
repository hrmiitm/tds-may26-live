# Day 1 Teaching Guide — Setup Day
## TDS Bridge Bootcamp

> **Who this is for:** Students who know Python (syntax, functions, lists, dicts) but have never seriously used a terminal or Linux.
> **Goal by end of session:** Student is comfortable in a Linux terminal, can navigate paths (including `~`, `.`, `..`, and `/mnt/c`), understands redirection (`>` vs `>>`), and can run `python`, `git`, and `uv` from inside a Linux shell.

---

## Section 1 — Why We Use Linux (10 min)

### The core idea

When you write Python, your code runs on some machine. In data science and engineering, that machine is almost always Linux — whether it's a cloud server, a Docker container, or a Kaggle GPU. If you only know how to work through Windows clicks, you'll be helpless the moment you need to debug something on a server, run a scheduled job, or install a package in a clean environment.

This isn't about preference. It's about speaking the language that the tools expect.

### Windows vs Linux — the practical differences you'll hit

| Thing | Windows | Linux / WSL |
|---|---|---|
| Path separator | `C:\Users\me\file.txt` | `/home/me/file.txt` |
| Case sensitivity | `File.txt` = `file.txt` | `File.txt` ≠ `file.txt` |
| Line endings | CRLF (`\r\n`) | LF (`\n`) |
| Where Python usually works best | Works, but messy | First-class citizen |


**Takeaway:** Paths and case sensitivity cause 80% of beginner confusion. Know this difference and you'll avoid most setup frustration.

### What is WSL2?

WSL2 (Windows Subsystem for Linux 2) runs a real Linux kernel *inside* Windows. It is not a virtual machine you have to manage separately — it's a terminal window that drops you inside Ubuntu. Everything you do there works exactly like real Linux.

**macOS users:** macOS is Unix-based, so your terminal already behaves like Linux. You don't need WSL.

---

**Section summary:** Linux is the standard environment for data science work. On Windows, WSL2 gives you a real Linux shell. Paths look different, and case matters.

---

## Section 2 — Setting Up Your Environment (40 min)

### Step 1: Install WSL2 + Ubuntu (Windows only)

Open **Windows PowerShell as Administrator** and run:

```powershell
wsl --install
```

This installs WSL2 and Ubuntu automatically. Restart when asked. Then open **Ubuntu** from the Start menu — you'll be inside Linux.

> **macOS / native Ubuntu:** skip this step; open your terminal directly.

### Step 2: Understand what just happened

After setup, your terminal prompt looks like this:

```
yourname@DESKTOP-XYZ:~$
```

Break it down:
- `yourname` — your Linux username
- `DESKTOP-XYZ` — your machine's name
- `~` — you are currently in your **home directory** (shorthand for `/home/yourname`)
- `$` — you are a regular user (not root/admin)

This prompt is your command line. Everything you type here is a command to Linux.

### Step 3: Navigate to your home directory

```bash
cd ~
pwd
```

`pwd` prints your current location. It will show something like `/home/yourname`. This is where all your personal files live. **This is where your TDS work should go.**

> **Windows + WSL note:** Your WSL home is `/home/yourname` inside Linux. Avoid storing files in `/mnt/c/...` (the Windows C: drive mounted in Linux) — it can be slow and causes permission headaches.

### Step 4: Create a folder for TDS work

```bash
mkdir -p ~/tds/bootcamp
cd ~/tds/bootcamp
pwd
```

You now have a clean workspace. This is where all your bootcamp work will live.

### Step 5: Install VS Code and connect it to WSL

1. Download and install **VS Code** from [code.visualstudio.com](https://code.visualstudio.com).
2. Install the **WSL extension** in VS Code (search "WSL" in the Extensions panel).
3. In your WSL terminal, navigate to your folder and run:

```bash
code .
```

VS Code opens with a green `WSL: Ubuntu` badge in the bottom-left corner. The terminal inside VS Code is now a Linux terminal.

> **macOS / Ubuntu:** `code .` works the same way after installing VS Code — no extension needed.

### Step 6: Install Git

Check if Git is already installed:

```bash
git --version
```

If not installed:

```bash
sudo apt update && sudo apt install git -y
```

### Step 7: Install uv (Python package and project manager)

`uv` is a modern, fast replacement for `pip` + `venv`. We use it for all Python work in TDS.

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

After install, close and reopen your terminal (or run `source ~/.bashrc`), then verify:

```bash
uv --version
```

### Step 8: Verify everything

Run these four commands and confirm each shows a version number:

```bash
python --version
git --version
uv --version
code --version
```

If any command says "not found", that tool is not installed. Call it out — we fix it together.

---

**Section summary:** WSL2 gives Windows users a real Linux shell. The home directory (`~`) is where your work lives. Four tools must be verified: Python, Git, uv, and VS Code. Run `code .` to open VS Code connected to Linux.

---

## Section 3 — The Filesystem Mental Model (15 min)

### Everything is a file (under a single tree)

In Linux, there is no `C:` drive or `D:` drive. Everything starts at `/` (called "root"). Your personal files live under `/home/yourname/`.

```
/
├── home/
│   └── yourname/          ← your home directory (~)
│       └── tds/
│           └── bootcamp/
├── etc/                   ← system configuration
├── usr/                   ← installed programs
└── tmp/                   ← temporary files
```

You don't need to understand the whole tree — just know that:
- `~` always means `/home/yourname`
- Your work goes in `~/tds/`
- Don't touch `/etc/` or `/usr/` for now

**What “everything is a file” means (practically):**
- Directories are a special kind of file.
- Your terminal input/output can be treated like files (that’s why `>` and `>>` work).
- Linux also exposes devices and system info as files (you’ll see this later in `/dev` and `/proc`).

You don’t need the theory today — just remember: in Linux, “files” aren’t only `.txt`.

### Windows drives inside WSL (`/mnt/c`, `/mnt/d`)

On Windows with WSL, your Windows drives are mounted inside Linux under `/mnt`:

```bash
ls /mnt
cd /mnt/c
pwd
```

If you have a D: drive, you’ll usually see `/mnt/d` too:

```bash
cd /mnt/d
pwd
```

> **Important:** You *can* access Windows files from WSL, but doing heavy development work in `/mnt/c` can be slower and cause permission quirks. Prefer working in `~/tds/`.

### Absolute vs relative paths

An **absolute path** starts from `/`. It works from anywhere:

```
/home/yourname/tds/bootcamp/notes.txt
```

A **relative path** starts from where you currently are. If you're in `~/tds/`:

```
bootcamp/notes.txt     ← relative path to the same file
```

The shorthand characters:
- `.` — current folder
- `..` — one folder up (parent)
- `~` — your home folder

### Three lookalikes: `.xyz`, `./xyz`, `../xyz`

These confuse almost everyone on Day 1:

- `.xyz` means a filename that *starts with a dot*. It’s usually hidden (e.g. `.bashrc`). It does **not** mean “current folder”.
- `./xyz` means “the file or folder named `xyz` inside the current folder”. (`./` = current directory)
- `../xyz` means “the file or folder named `xyz` inside the parent folder”. (`../` = one level up)

Example: if you are in `/home/alice/tds/bootcamp/`:

```bash
cat ./notes.txt      # notes.txt in this folder
cat ../notes.txt     # notes.txt in the parent folder (/home/alice/tds/)
touch .hidden-file
ls                  # may not show .hidden-file
ls -a               # will show .hidden-file
```

### Common navigation commands

```bash
pwd              # Where am I right now?
ls               # What's in this folder?
ls -la           # Same, but show hidden files and permissions
cd Documents     # Go into Documents (relative)
cd ~/tds         # Go to ~/tds (absolute)
cd ..            # Go one level up
cd -             # Go back to the previous folder
```

### Hidden files

Files starting with `.` are hidden (you won't see them with plain `ls`). Examples:
- `.bashrc` — shell configuration file (important!)
- `.gitignore` — tells Git which files to ignore

Use `ls -la` to see them.

---

**Section summary:** Linux has one file tree starting at `/`. Your work lives in `~` (home). Learn `pwd`, `ls`, and `cd` — they're the three commands you'll use the most. Paths can be absolute (start with `/`) or relative (start from where you are).

---

## Section 4 — Quick Terminal Confidence Exercise (20 min)

> This is a live, guided walkthrough. Do it together with students.

### The exercise

Run these commands one by one. After each one, ask: *"What did that do? What did you see?"*

```bash
# 1. Where are you?
pwd

# 2. Go home
cd ~

# 3. Create a folder structure
mkdir -p ~/tds/bootcamp/day-1

# 4. Go into it
cd ~/tds/bootcamp/day-1

# 5. Create a file
touch notes.txt

# 6. Confirm it exists
ls

# 7. Write something into it
echo "My first terminal command" > notes.txt

# 8. Read it back
cat notes.txt

# 9. Add another line (>> appends, > overwrites)
echo "This is Day 1 of TDS Bridge Bootcamp" >> notes.txt

# 10. Read it again
cat notes.txt

# 11. Open VS Code here
code .

# 12. (Windows + WSL) Jump to your Windows drive mounts
ls /mnt
cd /mnt/c
pwd

# 13. Go back home
cd ~
pwd
```

### Spot the difference: `>` vs `>>`

```bash
echo "Hello" > file.txt    # Creates (or OVERWRITES) the file
echo "World" >> file.txt   # APPENDS to the file
cat file.txt               # Shows: Hello\nWorld
```

This distinction matters a lot — `>` on an existing file destroys it.

---

**Section summary:** The terminal is just a way to talk to your computer in text. `echo`, `cat`, `touch`, `mkdir` are your first building blocks. The `>` and `>>` operators control where output goes.

---

## Section 5 — GitHub Account & First Repository (15 min)

### Why GitHub

GitHub stores your code remotely. For this bootcamp, it's your submission system — every day's work goes into a repository you push to GitHub.

### Steps

1. Go to [github.com](https://github.com) and create an account if you don't have one. Use a professional username (your name or initials — this will be visible to employers).

2. Create a new repository:
   - Name: `tds-bootcamp`
   - Visibility: Public
   - Add a README: Yes
   - Leave everything else as default

3. You now have a URL like `https://github.com/yourname/tds-bootcamp`. Keep this open.

> **We will configure Git and push to this repo on Day 5.** Today, just create the repo and confirm you can access it.

### Set your Git identity (do this now, once)

```bash
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

This is not authentication — it's just the name and email that appears in your commits. Do it once and forget it.

---

**Section summary:** GitHub is your remote storage for code. Create the `tds-bootcamp` repo today. Configure your Git name and email. We'll handle pushing and authentication on Day 5.

---

## Section 6 — Wrap Up & Common Problems (5 min)

### Frequently hit problems today

| Problem | Fix |
|---|---|
| `code` command not found | Open VS Code manually → Command Palette → "Install 'code' command in PATH" |
| `uv` not found after install | Run `source ~/.bashrc` or restart the terminal |
| `python` says version 2.x | Use `python3` instead, or `uv` will handle this for you |
| WSL terminal opens in `/mnt/c/...` | Run `cd ~` to get to your Linux home |
| Permission denied on `mkdir` | You're in a system folder; `cd ~` first |

### The one thing to remember

**Your work lives in `~/tds/`.** Every command you run today should be in that folder or below it. If you're confused about where you are, run `pwd`.

---

**End of Day 1**

> Remind students: fill in the post-session checklist honestly. Bring questions to Day 2.
