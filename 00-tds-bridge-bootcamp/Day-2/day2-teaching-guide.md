# Day 2 Teaching Guide — Linux & Shell Essentials
## TDS Bridge Bootcamp

> **Who this is for:** Students who completed Day 1 (terminal open, tools verified). Today they learn to actually *use* the terminal — not just open it.
> **Goal by end of session:** Student can navigate, read, search, edit files, chain commands with pipes, and set environment variables — entirely from the terminal.

---

## Section 1 — What Is a Shell, and What Is PATH? (10 min)

### The shell is just a program that reads your commands

When you open a terminal, you're running a program called a **shell**. The shell reads what you type, finds the right program to run, and shows you the result.

The most common shell is **Bash** (Bourne Again Shell). You can confirm which shell you're in:

```bash
echo $SHELL
```

You'll likely see `/bin/bash`. Other shells exist (`zsh` on macOS, `sh` for minimal scripts) but Bash is the standard for this bootcamp.

### PATH — how the shell finds your commands

When you type `python`, the shell doesn't magically know where Python is stored. It searches a list of folders called the **PATH**.

```bash
echo $PATH
```

Output looks like:
```
/home/alice/.local/bin:/usr/bin:/bin:/usr/local/bin
```

The shell checks each folder left-to-right until it finds an executable named `python`. If it finds nothing, you get "command not found".

This is why after installing `uv` yesterday, you had to run `source ~/.bashrc` — the install script added `uv`'s folder to PATH, but the current terminal hadn't loaded that update yet.

### Exit codes — did my command succeed?

Every command returns a number when it finishes. `0` means success. Anything else means failure.

```bash
ls /home          # succeeds
echo $?           # prints 0

ls /nonexistent   # fails
echo $?           # prints 2 (or some non-zero number)
```

`$?` always holds the exit code of the last command. Useful for debugging.

---

**Section summary:** A shell (usually Bash) reads your commands and runs programs. PATH tells the shell where to look for commands. Exit code `0` = success; anything else = failure.

---

## Section 2 — Filesystem Navigation (15 min)

### The five commands you'll use every day

```bash
pwd          # Where am I?
ls           # What's here?
ls -la       # Same, including hidden files and permissions
cd foldername  # Go into a folder
cd ..        # Go one level up
```

### Path shortcuts — these trip up beginners

| Symbol | Means |
|--------|-------|
| `~` | Your home directory (`/home/yourname`) |
| `.` | Current directory |
| `..` | Parent directory (one level up) |
| `-` (with cd) | The previous directory you were in |
| `./script.sh` | Run `script.sh` in the current directory |
| `.bashrc` | A hidden file (starts with `.`) in your home directory |

```bash
cd ~          # Go home
cd -          # Jump back to previous folder (handy!)
cd ../other   # Go up one level, then into 'other'
```

### Hidden files

Any file starting with `.` is hidden. They don't appear in plain `ls` — use `ls -la`.

```bash
ls -la ~      # Shows .bashrc, .profile, .ssh etc.
```

### `tree` — visualise your folder structure

```bash
tree ~/tds    # Shows a visual tree of all folders/files
```

If `tree` is not installed: `sudo apt install tree -y`

---

**Section summary:** `pwd`, `ls`, `cd` are your core trio. `.` is here, `..` is up, `~` is home, `-` is back. Hidden files start with `.` and need `ls -la` to appear.

---

## Section 3 — Reading and Inspecting Files (15 min)

### When `cat` is not enough

`cat` dumps an entire file to screen. Fine for small files, but useless for a 10,000-line log.

```bash
cat file.txt          # Print the whole file
head file.txt         # First 10 lines
head -n 20 file.txt   # First 20 lines
tail file.txt         # Last 10 lines
tail -n 5 file.txt    # Last 5 lines
tail -f server.log    # Live follow (great for watching running servers)
```

### Counting content

```bash
wc -l file.txt    # Count lines
wc -w file.txt    # Count words
wc -c file.txt    # Count bytes (characters)
```

### Searching inside files with `grep`

```bash
grep "error" file.txt           # Lines containing "error"
grep -i "error" file.txt        # Case-insensitive
grep -n "error" file.txt        # Show line numbers
grep -r "import" ~/tds/         # Search recursively in a folder
grep -v "debug" file.txt        # Lines that do NOT contain "debug"
```

> **Real use case:** You have a 500-line log file. You want only lines with `ERROR`:
> ```bash
> grep "ERROR" app.log
> ```

### `less` — scroll through big files

```bash
less bigfile.txt
```

Inside `less`: `↑↓` to scroll, `q` to quit, `/word` to search, `n` for next match.

---

**Section summary:** Use `head`/`tail` to peek at files, `wc` to count, `grep` to search, and `less` to browse large files without overwhelming your screen.

---

## Section 4 — Creating, Copying, Moving, Deleting (10 min)

### The essential file manipulation commands

```bash
mkdir foldername          # Create a folder
mkdir -p a/b/c            # Create nested folders in one go
touch file.txt            # Create an empty file (or update timestamp)
cp source.txt dest.txt    # Copy a file
cp -r folder/ copy/       # Copy a whole folder (-r = recursive)
mv old.txt new.txt        # Rename a file
mv file.txt ~/other/      # Move a file to another folder
rm file.txt               # Delete a file (NO recycle bin — gone forever)
rm -r folder/             # Delete a folder and everything inside
```

> ⚠️ **`rm` is permanent.** There is no Recycle Bin in Linux. Double-check before deleting.

### Editing files with nano

`nano` is the friendliest terminal text editor. You don't need to learn any special modes.

```bash
nano notes.txt
```

Inside nano:
- Type normally to edit
- `Ctrl+O` then `Enter` — Save
- `Ctrl+X` — Exit
- `Ctrl+K` — Cut a line
- `Ctrl+W` — Search

> **What about vim?** Vim is powerful but has a steep learning curve. Knowing how to exit it is enough for now: press `Esc`, then type `:q!` and press Enter.

---

**Section summary:** `mkdir`, `touch`, `cp`, `mv`, `rm` are your file management commands. Use `nano` for quick edits — it shows all shortcuts at the bottom. `rm` has no undo.

---

## Section 5 — Pipes and Redirection (20 min)

### The idea: connect commands like Lego bricks

Instead of saving intermediate results to files, you can feed the output of one command directly into the next using a **pipe** `|`.

```bash
command1 | command2 | command3
```

Each command processes what the previous one produced.

### Redirection: control where output goes

```bash
command > file.txt     # Send output to file (OVERWRITE)
command >> file.txt    # Send output to file (APPEND)
command 2> error.txt   # Send error messages to file
command 2>&1           # Merge errors into normal output
```

### Practical examples

**Count how many Python files are in a folder:**
```bash
ls ~/tds/ | grep ".py" | wc -l
```

**Find all error lines in a log and save them:**
```bash
grep "ERROR" app.log > errors.txt
```

**Count errors vs warnings in a log:**
```bash
grep -c "ERROR" app.log
grep -c "WARN" app.log
```

**Top 5 most frequent words in a file:**
```bash
cat file.txt | tr '[:upper:]' '[:lower:]' | tr -s ' ' '\n' | sort | uniq -c | sort -rn | head -5
```

Break it down step by step:
- `tr '[:upper:]' '[:lower:]'` — lowercase everything
- `tr -s ' ' '\n'` — split words onto separate lines
- `sort` — sort alphabetically (required before `uniq`)
- `uniq -c` — count consecutive duplicates
- `sort -rn` — sort by count, highest first
- `head -5` — take top 5

> This single pipeline replaces what would be a 15-line Python script. That's the power of pipes.

### `2>` — capturing errors separately

```bash
python script.py > output.txt 2> errors.txt
```

Normal output goes to `output.txt`. Error messages (stderr) go to `errors.txt`. Useful when running long jobs.

---

**Section summary:** Pipes (`|`) chain commands — each one feeds into the next. `>` overwrites a file, `>>` appends, `2>` captures errors. Pipelines replace short scripts for text processing.

---

## Section 6 — Environment Variables and `.bashrc` (15 min)

### What is an environment variable?

It's a named value that lives in your shell session and can be read by any program you run.

```bash
echo $HOME       # Your home directory
echo $USER       # Your username
echo $PATH       # The search path for commands
```

### Setting your own variables

```bash
# Set for this session only (lost when terminal closes)
MY_NAME="Alice"
echo $MY_NAME

# Make it available to programs you launch (child processes)
export MY_NAME="Alice"
```

The key difference:
- Without `export`: only the current shell sees the variable
- With `export`: the variable is passed to any program you run from this shell

### Making variables permanent with `.bashrc`

`.bashrc` is a script that runs every time you open a new Bash terminal. Add variables here to make them permanent.

```bash
nano ~/.bashrc
```

Add at the bottom:
```bash
export MY_API_KEY="your-key-here"
export TDS_DATA_DIR="$HOME/tds/data"
```

Save and exit (`Ctrl+O`, `Ctrl+X`). Then apply without restarting the terminal:

```bash
source ~/.bashrc
```

Now `echo $MY_API_KEY` works in any new terminal.

### Why this matters for TDS

API keys must never be hardcoded in Python files (they'd end up on GitHub). The correct pattern is:

```bash
# In .bashrc
export OPENAI_API_KEY="sk-..."
```

```python
# In your Python code
import os
api_key = os.environ["OPENAI_API_KEY"]
```

This keeps secrets out of your code.

---

**Section summary:** Environment variables are named values your shell and programs can read. Use `export` to share them with programs. Add them to `.bashrc` for permanence. Use `source ~/.bashrc` to reload without restarting. Never hardcode API keys — use environment variables instead.

---

## Section 7 — Getting Help from the Terminal (5 min)

You don't need to memorise every flag. Know how to look it up.

```bash
ls --help           # Quick flag reference (works for most commands)
man ls              # Full manual page (press q to exit)
```

`man` pages are comprehensive but dense. For friendlier explanations, `tldr` gives practical examples:

```bash
# Install tldr (optional)
sudo apt install tldr -y
tldr tar             # Shows the most common use cases for tar
```

> **Habit to build:** Before Googling a command, try `--help` first. It's faster and always accurate.

---

**Section summary:** `--help` gives quick usage. `man` gives the full manual. `tldr` (optional) gives practical examples. Build the `--help` habit before reaching for Google.

---

## Live Exercise — Do Together (15 min)

Run these commands step by step. Discuss what each one does.

```bash
# 1. Create a sample text file
cd ~/tds/bootcamp
echo "the cat sat on the mat" > sample.txt
echo "the cat ate a rat" >> sample.txt
echo "the dog sat on a log" >> sample.txt
echo "ERROR: disk full" >> sample.txt
echo "WARN: low memory" >> sample.txt
echo "ERROR: connection refused" >> sample.txt

# 2. Count total lines
wc -l sample.txt

# 3. Find only ERROR lines
grep "ERROR" sample.txt

# 4. Save errors to a separate file
grep "ERROR" sample.txt > errors.txt
cat errors.txt

# 5. Count errors
grep -c "ERROR" sample.txt

# 6. Top 3 most frequent words
cat sample.txt | tr '[:upper:]' '[:lower:]' | tr -s ' ' '\n' | sort | uniq -c | sort -rn | head -3

# 7. Edit the file with nano — add your name at the top
nano sample.txt

# 8. Set a variable and use it
export MY_SAMPLE="$HOME/tds/bootcamp/sample.txt"
cat $MY_SAMPLE
```

---

**End of Day 2**

> Remind students: fill in the post-session checklist and push `day2.md` to their GitHub repo.
