# Day 1 Quiz & Exercises
## TDS Bridge Bootcamp — Setup Day

> **Instructions:** Attempt the MCQs first (no Googling). Then do the exercises in your terminal. The goal is to *think* before you type.

---

## Part A — 30 Multiple Choice Questions

**Q1.** You open a terminal and see this prompt: `alice@laptop:~$`. What does the `~` mean?

- A) The terminal is connected to the internet
- B) You are in the root directory `/`
- C) You are in your home directory
- D) You are running as administrator

---

**Q2.** On Windows, a file path looks like `C:\Users\alice\notes.txt`. What is the equivalent Linux path for a file called `notes.txt` in alice's home directory?

- A) `C:/Users/alice/notes.txt`
- B) `/home/alice/notes.txt`
- C) `home\alice\notes.txt`
- D) `/alice/home/notes.txt`

---

**Q3.** You are in `/home/alice/projects/` and you run `cd ..`. Where are you now?

- A) `/home/`
- B) `/home/alice/`
- C) `/`
- D) `/home/alice/projects/..`

---

**Q4.** Which command tells you the full path of your current location?

- A) `ls`
- B) `where`
- C) `pwd`
- D) `cd`

---

**Q5.** You run `echo "hello" > output.txt` twice. What does `output.txt` contain?

- A) `hello` twice (two lines)
- B) `hello` once (the second run overwrites)
- C) An error — you cannot run echo twice
- D) The file is empty

---

**Q6.** What is the difference between `>` and `>>` in the terminal?

- A) `>` appends to a file; `>>` overwrites it
- B) `>` overwrites a file; `>>` appends to it
- C) They do the same thing
- D) `>>` is used only for error messages

---

**Q7.** On Linux, `File.txt` and `file.txt` are:

- A) The same file
- B) Two different files
- C) One overwrites the other
- D) Both invalid filenames

---

**Q8.** You want to list *all* files including hidden ones in your current directory. Which command do you use?

- A) `ls`
- B) `ls -l`
- C) `ls -la`
- D) `list --all`

---

**Q9.** What makes a file "hidden" in Linux?

- A) It has a `.hidden` extension
- B) Its filename starts with a dot (`.`)
- C) It is stored in the `/hidden/` folder
- D) It is owned by root

---

**Q10.** Which of these is an *absolute* path?

- A) `./notes.txt`
- B) `../projects/`
- C) `notes.txt`
- D) `/home/alice/notes.txt`

---

**Q11.** You are in `/home/alice/tds/` and run `cd ./bootcamp`. Where are you now?

- A) `/home/alice/bootcamp/`
- B) `/home/alice/tds/bootcamp/`
- C) `/bootcamp/`
- D) `/home/alice/tds/./bootcamp/` (an error)

---

**Q12.** What does WSL2 stand for, and what does it do?

- A) Web Server Layer 2 — runs websites locally
- B) Windows Subsystem for Linux 2 — runs a real Linux kernel inside Windows
- C) Windows Software Library 2 — manages installed software
- D) Workspace Shell Layer 2 — a special terminal for Windows

---

**Q13.** A student on Windows stores their TDS files at `/mnt/c/Users/alice/tds/`. Their instructor says to move them to `~/tds/`. Why?

- A) Files in `/mnt/c/` cannot be read by Python
- B) GitHub can't access `/mnt/c/`
- C) Working in `/mnt/c/` from WSL can be slow and cause permission issues
- D) `uv` doesn't work outside the home directory

---

**Q14.** After installing `uv`, you run `uv --version` and get "command not found". What is the most likely fix?

- A) Reinstall your operating system
- B) Run `source ~/.bashrc` or restart the terminal so the PATH updates
- C) Use `sudo uv --version`
- D) Install Python first

---

**Q15.** You run `git config --global user.name "Alice"`. What does this do?

- A) Logs you in to GitHub
- B) Sets the name that will appear in your Git commits on this machine
- C) Creates a new GitHub account called Alice
- D) Gives you admin rights in the repo

---

**Q16.** You run `mkdir -p ~/tds/bootcamp/day-1`. What does the `-p` flag do?

- A) Makes the folder private
- B) Creates all intermediate folders in the path if they don't exist
- C) Prints the created folder path
- D) Prevents overwriting an existing folder

---

**Q17.** Which command creates an empty file called `notes.txt`?

- A) `create notes.txt`
- B) `new notes.txt`
- C) `touch notes.txt`
- D) `echo notes.txt`

---

**Q18.** You open VS Code from your WSL terminal using `code .`. In the bottom-left corner of VS Code you see `WSL: Ubuntu`. What does this confirm?

- A) VS Code is connected to the internet
- B) VS Code is running inside the WSL Linux environment (not Windows)
- C) Ubuntu needs to be updated
- D) Your code will be uploaded to Ubuntu servers

---

**Q19.** The root directory in Linux is:

- A) `/home/`
- B) `~/`
- C) `/`
- D) `/root/`

---

**Q20.** A file saved on Windows has line endings `\r\n` (CRLF). You open it in Linux and notice Git complains. Why?

- A) Linux doesn't support text files
- B) The file is corrupted and needs to be re-downloaded
- C) Linux expects line endings `\n` (LF) only; CRLF causes differences Git tracks as changes
- D) Git on Linux cannot open files created on Windows

---

**Q21.** You see a file named `.env` in a project. What does the leading dot mean on Linux?

- A) It is a folder, not a file
- B) It is a hidden file by default
- C) It is an executable file
- D) It is a Windows-only file

---

**Q22.** You are in `/home/alice/tds/bootcamp/` and run `cat ./notes.txt`. What does `./` mean here?

- A) “Search everywhere on the computer”
- B) “Use the root directory”
- C) “Use the current directory”
- D) “Use the parent directory”

---

**Q23.** You are in `/home/alice/tds/bootcamp/` and run `cat ../notes.txt`. Where does it look for `notes.txt`?

- A) In `/home/alice/tds/`
- B) In `/home/alice/tds/bootcamp/`
- C) In `/home/alice/`
- D) In `/notes.txt`

---

**Q24.** Which of the following is a correct statement about `.xyz`, `./xyz`, and `../xyz`?

- A) `.xyz` always means “current directory”
- B) `./xyz` refers to a file/folder named `xyz` in the current directory
- C) `../xyz` refers to a hidden file called `xyz`
- D) They are three different ways to write the same thing

---

**Q25.** In WSL on Windows, where is your Windows C: drive typically mounted?

- A) `/windows/c`
- B) `/drives/c`
- C) `/mnt/c`
- D) `~/c`

---

**Q26.** You want to check which Windows drives are available inside WSL. What is the best first command?

- A) `ls /mnt`
- B) `ls /home`
- C) `ls /windows`
- D) `pwd /mnt`

---

**Q27.** Which statement best matches “in Linux, everything is a file”?

- A) Only files with extensions like `.txt` count as files
- B) Many system resources (including devices and directories) are represented through the filesystem
- C) Linux cannot store files bigger than memory
- D) Files are stored only in `/home`

---

**Q28.** You run `cd /mnt/d` and get “No such file or directory”. What is the most likely explanation?

- A) WSL is broken and must be reinstalled
- B) Your machine probably doesn’t have a D: drive mounted (or it uses a different mount)
- C) You need to run `sudo cd /mnt/d`
- D) `cd` cannot be used with `/mnt`

---

**Q29.** You are in `~/tds/bootcamp/` and run `ls ../`. What are you listing?

- A) The contents of your current folder
- B) The contents of the parent folder (`~/tds/`)
- C) The contents of the root folder (`/`)
- D) Hidden files only

---

**Q30.** You run `touch .xyz` and then `ls` but you can’t see the file. Why?

- A) `touch` failed because filenames cannot start with a dot
- B) `ls` hides dotfiles by default; use `ls -a` or `ls -la`
- C) The file was created in `/tmp`
- D) Hidden files only appear after restarting the terminal

---

## Answer Key

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | C | `~` is the shorthand for your home directory |
| 2 | B | Linux home is `/home/username/` |
| 3 | B | `..` goes one level up; from `projects/` that's `alice/` |
| 4 | C | `pwd` = Print Working Directory |
| 5 | B | `>` overwrites; second run replaces the file |
| 6 | B | `>` overwrites, `>>` appends |
| 7 | B | Linux is case-sensitive; two different files |
| 8 | C | `-a` shows hidden files; `-l` shows long format |
| 9 | B | Any filename starting with `.` is hidden |
| 10 | D | Absolute paths start with `/` |
| 11 | B | `./bootcamp` means `bootcamp` inside current directory |
| 12 | B | WSL2 runs a real Linux kernel inside Windows |
| 13 | C | `/mnt/c/` is slow and has permission quirks from WSL |
| 14 | B | PATH updates when you source `.bashrc` or restart |
| 15 | B | Git identity for commit authorship, not authentication |
| 16 | B | `-p` creates parent directories as needed |
| 17 | C | `touch` creates an empty file |
| 18 | B | The green badge confirms the WSL connection |
| 19 | C | `/` is the root of the entire Linux filesystem |
| 20 | C | CRLF vs LF is a real difference; Git may flag it |
| 21 | B | Filenames starting with `.` are hidden by default |
| 22 | C | `./` is the current directory |
| 23 | A | `..` means “one level up” from `bootcamp/` |
| 24 | B | `./xyz` is `xyz` in the current directory |
| 25 | C | WSL mounts Windows drives under `/mnt` |
| 26 | A | `/mnt` is where drive mounts show up in WSL |
| 27 | B | Linux exposes lots of “things” via the filesystem |
| 28 | B | Not everyone has a mounted D: drive in WSL |
| 29 | B | `../` refers to the parent directory |
| 30 | B | Dotfiles are hidden from plain `ls` |

---

## Part B — Small Terminal Exercises

> Do these in your terminal after the session. Each takes 3–5 minutes.

---

### Exercise 1 — Explore and navigate

```bash
# 1. Print your current directory
pwd

# 2. Go to your home directory
cd ~

# 3. List everything including hidden files
ls -la

# 4. How many items do you see? (count the lines)
```

**Question to answer:** What hidden files or folders do you see? Write down at least two.

---

### Exercise 2 — Build a folder structure

Without using a file manager (mouse), create this structure using only the terminal:

```
~/tds/
├── bootcamp/
│   └── day-1/
│       └── notes.txt
└── practice/
```

Commands to use: `mkdir -p`, `touch`, `ls`

After creating it, run:
```bash
ls ~/tds/
ls ~/tds/bootcamp/day-1/
```

---

### Exercise 3 — Write and read files

```bash
cd ~/tds/bootcamp/day-1/

# Write your name into notes.txt
echo "My name is [YOUR NAME]" > notes.txt

# Check what's in it
cat notes.txt

# Append a second line
echo "Today is Day 1 of TDS Bridge Bootcamp" >> notes.txt

# Read the whole file again
cat notes.txt
```

**Expected output:**
```
My name is [YOUR NAME]
Today is Day 1 of TDS Bridge Bootcamp
```

---

### Exercise 4 — Verify your tools

Run each of these and write down the output you see:

```bash
python --version
git --version
uv --version
code --version
```

If any fails with "command not found", note it down and bring it to Day 2.

---

### Exercise 5 — Spot the path type

Label each path below as **Absolute** or **Relative**:

1. `/home/alice/projects/app.py` → ___________
2. `./scripts/run.sh` → ___________
3. `../data/input.csv` → ___________
4. `~/tds/bootcamp/` → ___________  *(bonus: is `~` absolute or relative? Think about it.)*
5. `notes.txt` → ___________

> **Bonus thinking question:** `~` expands to `/home/yourname`. Does that make it absolute or relative? Discuss with a classmate.

---

### Exercise 6 — (Windows + WSL) Navigate to C: and D: drives

1. List available drive mounts:

```bash
ls /mnt
```

2. Go to your Windows C: drive:

```bash
cd /mnt/c
pwd
ls
```

3. If you see a `d` entry in `/mnt`, try:

```bash
cd /mnt/d
pwd
ls
```

4. Return to your Linux home:

```bash
cd ~
pwd
```

**Question to answer:** Why do we recommend doing day-to-day coding work in `~/tds/` instead of `/mnt/c/...`?

---

### Exercise 7 — Practice `.xyz` vs `./xyz` vs `../xyz`

Run these commands and explain what each one is referencing:

```bash
cd ~/tds/bootcamp/day-1

touch .xyz
echo "current folder" > xyz
echo "parent folder" > ../parent.txt

ls
ls -la

cat ./xyz
cat ../parent.txt
```

**Questions to answer:**

1. Why does `ls` not show `.xyz` but `ls -la` does?
2. What’s the difference between `.xyz` and `./xyz`?
3. What does `../` mean in a path?

---

*End of Day 1 Quiz & Exercises*
