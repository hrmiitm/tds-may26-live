# 🐧 Master the Terminal: CLI & Pipe Chaining

**Welcome to your CLI foundational refresher!** 

The terminal can feel intimidating, but it is simply a text-based interface for interacting with your machine. By learning to link commands, you can perform complex operations in seconds that would take minutes using a mouse. 


🎬 **Watch This First:** [Terminal For Beginners (YouTube)](https://youtu.be/iwolPf6kN-k) 

---

## 🌱 The Absolute Basics: What is the Terminal?

Normally, you interact with your computer using a **Graphical User Interface (GUI)** by pointing and clicking. The **Terminal** (or Command Line Interface - **CLI**) removes the graphics. Instead of clicking a folder, you type a command. This gives you more power, speed, and precision.

### 🌳 The Filesystem Tree
Think of your computer's files like an upside-down tree.

* **Root (`/`):** The very top of the tree where everything begins. All other directories branch off from here.
* **Home (`~`):** Your personal user folder where your projects and personal files live.
* **Absolute Path:** The full address starting from the root (e.g., `/home/user/documents/file.txt`). It always starts with a `/`.
* **Relative Path:** The address starting from where you currently are (e.g., `documents/file.txt` or `./documents/file.txt`).

---

## ✅ Module Checklist

* [ ] Understand the difference between GUI and CLI.
* [ ] Grasp absolute vs. relative file paths.
* [ ] Master basic navigation (`pwd`, `ls`, `cd`).
* [ ] Know how to chain commands with the pipe (`|`).
* [ ] Know how to save output with redirects (`>` and `>>`).

---

## 🧠 The Core Concept: Piping and Redirection

Every Linux command works like a factory machine. It takes raw material (**Standard Input / `stdin`**), processes it, and produces a result (**Standard Output / `stdout`**). If it fails, it produces an error message (**Standard Error / `stderr`**).

1. **The Pipe (`|`) - The Conveyor Belt:**
   It takes the successful output (`stdout`) of the *first* command and feeds it directly into the *second* command as input (`stdin`). This allows you to chain multiple simple commands to perform complex tasks.
   
   *Example:* `ls -la | grep "config"` 
   *(Lists all files, then pipes that list to `grep`, which filters for the word "config".)*

2. **The Redirect (`>`) - The Delivery Truck (Overwrite):** 
   It takes the final output and drops it off into a text file. **Warning:** This will completely overwrite the file if it already exists!
   
   *Example:* `echo "Hello" > file.txt`

3. **The Appender (`>>`) - The Safe Delivery Truck (Append):** 
   Adds data to the bottom of an existing file safely, without erasing what was already there.
   
   *Example:* `echo "World" >> file.txt`

---

## 🏋️ Practice Exercises & Solutions

Test your skills with these practical scenarios. Try to solve them before peeking at the answers!

### Exercise 1: The Navigator & Creator
Find out your current exact directory path. Create a new directory called `cli_practice`, navigate into it, and create an empty file named `data.txt`. Verify the file was created.

<details>
<summary>💡 Click to reveal the solution</summary>

```bash
# Print working directory
pwd

# Make directory and move into it
mkdir cli_practice
cd cli_practice

# Create empty file
touch data.txt

# Verify it was created
ls
```
</details>

### Exercise 2: The Data Pipeline
Using the `echo` command, write the following three lines into `data.txt` (making sure you don't overwrite previous lines!): "apple", "banana", and "apple pie". Check the contents of the file when you're done.

<details>
<summary>💡 Click to reveal the solution</summary>

```bash
# Overwrite or create file with the first line
echo "apple" > data.txt

# Append remaining lines
echo "banana" >> data.txt
echo "apple pie" >> data.txt

# Check the contents
cat data.txt
```
</details>

### Exercise 3: The Filter
Read the contents of `data.txt`. Pipe that output into the `grep` command to search *only* for lines containing the word "apple". Finally, redirect that filtered output into a new file called `filtered.txt`.

<details>
<summary>💡 Click to reveal the solution</summary>

```bash
# Chain cat, grep, and redirect
cat data.txt | grep "apple" > filtered.txt

# Verify the result
cat filtered.txt
```
</details>

---

## 🤔 Conceptual Questions

1. **What is the danger of using `>` instead of `>>` when trying to update an important system log?**
<details>
<summary>💡 Answer</summary>
Using `>` will completely overwrite the system log, deleting all historical log data. You should always use `>>` when adding new entries to a log file to preserve the existing content.
</details>

2. **If you run `ls /fake_directory > output.txt`, why does the error message print to your screen instead of going into the text file?**
<details>
<summary>💡 Answer</summary>
The `>` operator only redirects Standard Output (`stdout`). Error messages are sent to a different stream called Standard Error (`stderr`). Because `stderr` is not being redirected, it still prints to your screen. *(Bonus: To redirect stderr as well, you would use `2>` or `2>&1`!)*
</details>

---

## 📝 CLI Quick Reference Cheat Sheet

| Command / Symbol | Description | Example |
| :--- | :--- | :--- |
| `pwd` | **P**rint **W**orking **D**irectory (shows current absolute path) | `pwd` |
| `ls` | **L**i**s**t directory contents | `ls -la` |
| `cd` | **C**hange **D**irectory | `cd /var/log` |
| `touch` | Create an empty file (or update timestamp) | `touch newfile.txt` |
| `mkdir` | **M**a**k**e **Dir**ectory | `mkdir my_folder` |
| `cat` | Con**cat**enate and display file contents | `cat my_file.txt` |
| `echo` | Print text to the terminal | `echo "Hello World"` |
| `grep` | Search text using patterns | `grep "error" app.log` |
| `|` (Pipe) | Send output of command A as input to command B | `ls | grep "txt"` |
| `>` (Redirect) | Write output to a file (overwrites!) | `echo "hi" > file.txt` |
| `>>` (Append) | Add output to the end of a file | `echo "more" >> file.txt` |
| `~` | Tilde shorthand for your Home directory | `cd ~` or `cd ~/Downloads` |
| `.` | Represents the current directory | `./script.sh` |
| `..` | Represents the parent directory (one level up) | `cd ..` |
