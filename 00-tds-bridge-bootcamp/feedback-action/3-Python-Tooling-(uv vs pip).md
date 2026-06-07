
### Module 3: Python Tooling (`uv` vs `pip`)

# ⚡ Next-Gen Python: Tooling & Environments

**Welcome to your modern Python refresher!** The Python ecosystem is evolving rapidly. While `pip` and `venv` are the classics, the industry is moving toward all-in-one, blazingly fast tools like `uv`. Let's understand why this shift is happening.

🎬 **Watch This First:** [Astral UV: The Future of Python Tooling (YouTube)](https://www.google.com/search?q=https://www.youtube.com/results%3Fsearch_query%3DAstral%2BUV%2BThe%2BFuture%2Bof%2BPython%2BTooling)
📖 **Read More:** [Astral `uv` Official Documentation](https://docs.astral.sh/uv/)

### 🌱 The Absolute Basics: Why do we need Environments?

Python doesn't have every tool built-in[cite: 3]. Developers rely on 'Packages' (third-party code libraries) like `requests` or `fastapi`[cite: 3].

If you use `pip install` globally, packages are installed into your main "System Python"[cite: 3]. This is dangerous. Project A might require `django==3.0`, while Project B requires `django==4.0`[cite: 3]. If they share the System Python, one project will inevitably break.
**The Solution:** Virtual Environments (`.venv`). These are isolated, mini-Python installations specific to just one project[cite: 3].

### ✅ Module Checklist

* [ ] Understand the dangers of "dependency hell" and global package installations[cite: 3].
* [ ] Grasp what a Virtual Environment is[cite: 3].
* [ ] Learn the drawbacks of traditional tools (`pip` + `venv`)[cite: 3].
* [ ] Master modern environment and dependency setup with `uv`[cite: 3].
* [ ] Understand the importance of `pyproject.toml` and lockfiles (`uv.lock`)[cite: 3].

### 🧠 The Core Concept: Modern Tooling with `uv`

Historically, Python project setup was tedious. You had to run `python -m venv .venv`, remember the exact command to activate it, use `pip install`, and manually freeze dependencies into a `requirements.txt` file[cite: 3].

**`uv` replaces all of this with a unified workflow.** Written in Rust, it is incredibly fast.

* `uv init` scaffolds your project automatically[cite: 3].
* `uv add <package>` automatically handles the virtual environment creation, installs the package, and updates a `pyproject.toml` file[cite: 3].
* **The Lockfile:** `uv` generates a `uv.lock` file. A lockfile guarantees deterministic, reproducible builds[cite: 3]. If your teammate clones your project, the lockfile ensures they get the exact same environment—down to the last sub-dependency—preventing the dreaded "it works on my machine" problem[cite: 3].

### 🏋️ Practice Exercises & Solutions

**Exercise 1: Start Fresh**
Create a new folder called `weather_app`, navigate into it, and initialize a new `uv` project. Observe the files it generates.

**Exercise 2: Add Dependencies**
Use `uv` to add the `requests` library. Check the contents of the `pyproject.toml` file to see how it was recorded.

**Exercise 3: Run Code Seamlessly**
Create a `main.py` file that imports `requests` and prints the status code of a Google homepage request. Run the file safely inside the isolated environment using `uv run`.

```bash
# Solution 1: Setup
mkdir weather_app
cd weather_app
uv init
ls -la # Notice pyproject.toml is created

# Solution 2: Add a package
uv add requests
cat pyproject.toml # Look under [project.dependencies]

# Solution 3: Run your script safely
cat << 'EOF' > main.py
import requests
response = requests.get("https://www.google.com")
print(f"Google responded with status code: {response.status_code}")
EOF

# uv run automatically handles the virtual environment execution!
uv run main.py

```

### 🤔 Conceptual Questions

1. If you delete your virtual environment folder (`.venv`), how does `uv` know exactly what to reinstall when you run `uv sync`?
2. What is the primary advantage of a `uv.lock` file over a standard `requirements.txt` file when deploying to production?

---