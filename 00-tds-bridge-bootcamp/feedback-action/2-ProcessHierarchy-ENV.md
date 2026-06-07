
### Module 2: Process Hierarchy & Environment Variables

# 🌳 Shell Inception: Process Hierarchy & Variables

**Welcome to your Process Hierarchy refresher!** Have you ever wondered why a variable you create in the terminal vanishes when you close the window, or why configuring `.bashrc` is necessary? It all comes down to how Linux manages processes.

🎬 **Watch This First:** [Understanding Linux Environment Variables & .bashrc (YouTube)](https://www.google.com/search?q=https://www.youtube.com/results%3Fsearch_query%3DUnderstanding%2BLinux%2BEnvironment%2BVariables%2Band%2Bbashrc)
📖 **Read More:** [DigitalOcean: How to Read and Set Environmental Variables](https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-linux)

### 🌱 The Absolute Basics: What is a Process?

Every time you run a program or type a command in Linux, the system creates a **Process**[cite: 4]. A process is simply a running instance of a program[cite: 4]. Every process is assigned a unique ID number called a **PID** (Process ID)[cite: 4].

When you open your terminal application, you are starting a process called a **Shell** (usually `bash` or `zsh`)[cite: 4].

### ✅ Module Checklist

* [ ] Understand what a Process and a PID are[cite: 4].
* [ ] Grasp the relationship between Parent and Child processes[cite: 4].
* [ ] Know the difference between local shell variables and environment variables[cite: 4].
* [ ] Master the `export` command[cite: 4].
* [ ] Make variables and aliases permanent using `.bashrc`[cite: 4].

### 🧠 The Core Concept: Parent vs. Child Processes

Imagine your current terminal as a large cardboard box (the **Parent Process**)[cite: 4]. If you type the command `bash`, you launch a new shell *inside* the current one. You just placed a smaller box (the **Child Process**) inside the big box[cite: 4].

* **Local Variables:** If you create a variable (like `SECRET="hidden"`) inside the big box, the smaller box cannot see it[cite: 4]. It is localized strictly to the parent.
* **Environment Variables (`export`):** The `export` command is like cutting a window in the big box[cite: 4]. It promotes a local variable into an 'Environment Variable', meaning any child processes created afterward can now inherit, see, and use that variable[cite: 4].
* **Persistence (`.bashrc`):** Because all variables are destroyed in RAM when you close the terminal, we write our `export` commands into a special startup script called `~/.bashrc` (or `~/.zshrc`)[cite: 4]. The terminal reads this file every single time it opens, automatically recreating your variables[cite: 4].

### 🏋️ Practice Exercises & Solutions

**Exercise 1: The Trapped Variable**
Create a local variable `API_KEY="12345"`. Verify it exists by echoing it. Open a sub-shell by typing `bash`. Try to echo `$API_KEY` again. Then, type `exit` to return to the parent shell.

**Exercise 2: The Exported Variable**
Now, use the `export` command on `API_KEY`. Open a sub-shell again (`bash`), and try echoing it. It should now be visible! Type `exit` to return.

**Exercise 3: The Permanent Shortcut**
We want a command called `update_sys` that automatically runs `sudo apt update`. Add this alias to your `.bashrc` file using the terminal, reload the file, and test it.

```bash
# Solution 1: The Trapped Variable
API_KEY="12345"
echo $API_KEY   # Outputs: 12345
bash            # Enters child process
echo $API_KEY   # Outputs nothing!
exit            # Returns to parent

# Solution 2: The Exported Variable
export API_KEY="12345"
bash            # Enters child process
echo $API_KEY   # Outputs: 12345
exit            # Returns to parent

# Solution 3: The Permanent Shortcut
echo 'alias update_sys="sudo apt update"' >> ~/.bashrc
source ~/.bashrc  # Reloads the configuration into the current shell
update_sys

```

### 🤔 Conceptual Questions

1. The `$PATH` variable is one of the most important environment variables in Linux. What does it do, and what happens if you accidentally overwrite it entirely?
2. If a child process modifies an `export`ed variable, does the parent process see the change? *(Hint: Variables are passed down, not up!)*

---