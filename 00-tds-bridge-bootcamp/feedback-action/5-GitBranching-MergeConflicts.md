### Module 5: Git Branching & Merge Conflicts

# 🔀 Time Travel & Timelines: Git Branching

**Welcome to your Git Branching refresher!** The true power of Git isn't just backing up your code—it’s the ability to create alternate, safe timelines (branches) to experiment without fear of breaking the main project.

🎬 **Watch This First:** [Git Branching and Merge Conflicts Visually Explained (YouTube)](https://www.google.com/search?q=https://www.youtube.com/results%3Fsearch_query%3DGit%2BBranching%2Band%2BMerge%2BConflicts%2BVisually%2BExplained)
📖 **Read More:** [Atlassian Git Branching Tutorial](https://www.atlassian.com/git/tutorials/using-branches)

### 🌱 The Absolute Basics: What is Version Control?

Before Git, if you wanted to save versions of a project, you likely named folders like this: `Project`, `Project_Final`, `Project_Final_v2`, `Project_Final_REAL`[cite: 1]. This is chaotic and impossible to scale across a team.

Version Control Systems (like Git) solve this by tracking the exact history of your project over time[cite: 1]. Instead of copying whole folders, Git takes a lightweight "Snapshot" (called a **Commit**) of your file changes at a specific point in time[cite: 1]. You can safely rewind back to any commit whenever you want.

### ✅ Module Checklist

* [ ] Understand why Version Control is superior to manual copying[cite: 1].
* [ ] Grasp the three stages: Working Directory, Staging Area, and Repository.
* [ ] Understand Branching (creating alternate timelines)[cite: 1].
* [ ] Understand exactly what causes a Merge Conflict[cite: 1].
* [ ] Safely identify, edit, and resolve a basic conflict[cite: 1].

### 🧠 The Core Concept: Branching and Merge Conflicts

Imagine you are writing a book. The `main` branch is your official, published draft[cite: 1].

* **Branching:** You want to write an experimental ending without ruining the published book. So, you tell Git to create a **Branch**. A branch is an alternate timeline[cite: 1]. You make changes safely. If you like them, you **Merge** the branch back into the main timeline[cite: 1].
* **Merge Conflicts:** If you edit Page 10 in your experimental branch, AND a teammate edits the *exact same sentence* on Page 10 in the `main` branch, Git gets confused when trying to merge them[cite: 1]. It stops and says: *"Hey, you both edited the exact same line! I don't know which one to keep."*[cite: 1]. This is a **Merge Conflict**[cite: 1]. You must manually open the file, delete Git's warning markers (`<<<<<<<`), and save the correct text[cite: 1].

### 🏋️ Practice Exercises & Solutions

**Exercise 1: The Alternate Timeline**
Initialize a new Git repository. Add a file called `story.txt` with the text "Chapter 1", and commit it. Create a new branch called `alternate-ending` and switch to it.

**Exercise 2: Setup for Disaster**
On the `alternate-ending` branch, change the text in `story.txt` to "Chapter 1 - The hero dies". Commit it. Switch back to your `main` branch. Change `story.txt` to "Chapter 1 - The hero lives". Commit it.

**Exercise 3: The Resolution**
Try to merge the `alternate-ending` branch into `main`. Git will warn you of a conflict. Open the file, resolve the conflict so the hero lives, and make the final merge commit.

```bash
# Solution 1: Setup and Branch
mkdir git_practice && cd git_practice
git init
echo "Chapter 1" > story.txt
git add .
git commit -m "Initial commit"
git checkout -b alternate-ending

# Solution 2: Create the conflict
echo "Chapter 1 - The hero dies" > story.txt
git commit -am "Dark ending"
git checkout main
echo "Chapter 1 - The hero lives" > story.txt
git commit -am "Happy ending"

# Solution 3: The Merge and Fix
git merge alternate-ending
# Git will output: CONFLICT (content): Merge conflict in story.txt

# Open story.txt in your editor. It will look like this:
# <<<<<<< HEAD
# Chapter 1 - The hero lives
# =======
# Chapter 1 - The hero dies
# >>>>>>> alternate-ending

# Delete the markers and the dark ending so only "Chapter 1 - The hero lives" remains. Save it.
git add story.txt
git commit -m "Resolved conflict: Kept happy ending"

# View your beautiful timeline!
git log --graph --oneline

```

### 🤔 Conceptual Questions

1. What does the `git add` command actually do? (What is the "Staging Area"?)
2. Does creating a new branch duplicate all of your files on your physical hard drive?