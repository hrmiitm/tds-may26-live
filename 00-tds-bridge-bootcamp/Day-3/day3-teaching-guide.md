# Day 3 Teaching Guide — VS Code + Python Projects with uv
## TDS Bridge Bootcamp

> **Who this is for:** Students who are comfortable in the terminal (Day 2 done). They know Python syntax but have always used Jupyter or IDLE. They've never thought about *how* a project is set up.
> **Goal by end of session:** Student can create a clean Python project with `uv`, add dependencies, run scripts, and work comfortably in VS Code.

---

## Section 1 — The Problem with `pip install` Everywhere (10 min)

### What most beginners do (and why it breaks)

Most Python beginners install packages like this:

```bash
pip install pandas
pip install numpy
pip install requests
```

This installs everything into one global location. The first few days it's fine. Then this happens:

- **Project A** needs `pandas==1.5` (old version, specific behaviour)
- **Project B** needs `pandas==2.1` (new version, changed API)
- You install `pandas==2.1` for Project B — now Project A breaks

There's no way to have two different versions of the same package globally. This is called **dependency conflict** and it's the #1 frustration for Python beginners working on multiple projects.

### The solution: isolated environments per project

The idea is simple — **each project gets its own Python installation box**. Packages installed in Project A's box don't touch Project B's box. They're completely isolated.

This is what a **virtual environment** is.

```
~/tds/
├── project-a/
│   └── .venv/          ← Project A's isolated Python
│       └── pandas 1.5
└── project-b/
    └── .venv/          ← Project B's isolated Python
        └── pandas 2.1
```

They don't interfere. This is the correct way to work with Python.

---

**Section summary:** Global `pip install` causes version conflicts across projects. Virtual environments isolate each project's packages. This is not optional — it's how professional Python development works.

---

## Section 2 — Why uv? (5 min)

`uv` is a modern Python package manager that replaces the combination of `pip` + `venv` + `pip-tools`. It's:

- **Fast** — installs packages 10–100× faster than pip
- **Simple** — one tool instead of three
- **Reproducible** — creates a lockfile so teammates get identical environments
- **Standard** — uses `pyproject.toml`, the modern Python project format

You don't need to know all of this deeply. Just know: **use `uv` for everything Python in TDS. Don't touch `pip` directly.**

---

**Section summary:** `uv` replaces `pip` + `venv` + `pip-tools` in one fast, simple tool. Always use `uv` in TDS projects.

---

## Section 3 — Creating a Project with uv (25 min)

### Method A — Full project (recommended for anything beyond a single script)

```bash
cd ~/tds/bootcamp
uv init day-3-project
cd day-3-project
```

What `uv init` creates:

```
day-3-project/
├── pyproject.toml    ← project metadata + dependencies
├── .python-version   ← which Python version this project uses
├── README.md
└── hello.py          ← starter script
```

### Understanding `pyproject.toml`

Open it:

```toml
[project]
name = "day-3-project"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = []
```

This is the modern Python project file. It replaces the old `requirements.txt`. When you add packages, they appear in `dependencies`. Anyone who clones your project can recreate the exact same environment.

### Adding packages

```bash
uv add requests
uv add pandas numpy
```

After running this:
- Packages are installed in a hidden `.venv/` folder inside your project
- `pyproject.toml` now lists them under `dependencies`
- A `uv.lock` file is created — this freezes exact versions for reproducibility

### Running your script

```bash
uv run hello.py
```

`uv run` automatically uses the project's virtual environment. You never need to "activate" anything.

### Method B — Traditional venv (useful to understand)

Sometimes you need the classic approach (for compatibility with older tools or tutorials):

```bash
uv venv              # Creates .venv/ in current folder
source .venv/bin/activate   # Activate (prompt changes to show venv name)
uv pip install requests     # Install into the active venv
python script.py     # Run normally
deactivate           # Exit the venv
```

> **Which to use?** For all TDS work: Method A (`uv init` + `uv run`). Only use Method B if a tutorial or tool specifically asks for it.

### Key uv commands summary

| Command | What it does |
|---------|--------------|
| `uv init myproject` | Create a new project folder with `pyproject.toml` |
| `uv add requests` | Add a package to the project |
| `uv remove requests` | Remove a package |
| `uv run script.py` | Run a script using the project's environment |
| `uv sync` | Install all packages listed in `pyproject.toml` (e.g. after cloning) |
| `uv venv` | Create a plain virtual environment (Method B) |
| `uv pip install x` | Install into the currently active venv (Method B only) |

---

**Section summary:** `uv init` creates a project. `uv add` installs packages. `uv run` executes scripts. You never need to manually activate anything. `uv sync` restores a teammate's environment from the lock file.

---

## Section 4 — VS Code for Python Projects (20 min)

### Opening your project

From the terminal, inside your project folder:

```bash
code .
```

VS Code opens with your project in the file explorer on the left.

### The integrated terminal

Press `` Ctrl+` `` (backtick) or go to **Terminal → New Terminal**. This opens a terminal *inside* VS Code, in your project folder. You don't need a separate terminal window.

### Selecting the Python interpreter

VS Code needs to know which Python to use — specifically, the one inside your project's `.venv/`.

1. Press `Ctrl+Shift+P` to open the Command Palette
2. Type `Python: Select Interpreter`
3. Choose the one that shows `.venv` in its path (e.g. `~/tds/bootcamp/day-3-project/.venv/bin/python`)

Once selected, VS Code uses this interpreter for running, linting, and IntelliSense.

### Essential extensions to install

Open the Extensions panel (`Ctrl+Shift+X`) and install:

| Extension | Why |
|-----------|-----|
| **Python** (Microsoft) | Core Python support, running, debugging |
| **Pylance** | Fast type checking and autocompletion |
| **Jupyter** | Run `.ipynb` notebooks in VS Code |

### Useful VS Code features

**Multi-cursor editing:** Hold `Alt` and click in multiple places to type in all of them simultaneously. Or `Ctrl+D` to select the next occurrence of a word.

**Search across files:** `Ctrl+Shift+F` — search for any text across every file in the project. Incredibly useful when you forget where you defined a function.

**File explorer:** The left sidebar shows your project files. Click any file to open it. Right-click for rename/delete.

**Formatting:** VS Code can auto-format your Python code. Press `Shift+Alt+F`. If asked to select a formatter, choose **Ruff** (if installed) or **Black** — both enforce consistent style.

> **Ruff/Black conceptually:** These tools reformat your code to follow a consistent style (indentation, line length, spacing). You don't need to install them today — just know they exist and that TDS uses them.

---

**Section summary:** Open projects with `code .`. Use the integrated terminal so you stay in VS Code. Select the `.venv` interpreter via Command Palette. Install Python + Pylance + Jupyter extensions. `Ctrl+Shift+F` finds anything across your whole project.

---

## Section 5 — Scripts vs Notebooks (10 min)

### `.py` scripts — use when

- Running something repeatedly (a data pipeline, a CLI tool, a service)
- The code needs to be imported by other files
- You want clean, testable, version-controllable code
- Running in production or on a server

```bash
uv run main.py    # Runs start to finish, once
```

### `.ipynb` notebooks — use when

- Exploring data interactively (you want to see output cell by cell)
- Writing a report that mixes code + explanation + charts
- Prototyping — you want to run just part of the code

```bash
# Open a notebook in VS Code
# Just create a file called analysis.ipynb and VS Code handles it
```

### The workflow in practice

Most data science work goes like this:
1. **Explore in a notebook** — try things, see what the data looks like
2. **Move working code into `.py` scripts** — clean it up, make it reusable
3. **Run scripts from terminal** — automated, reproducible

> A common mistake: writing everything in notebooks and never moving to scripts. Notebooks are hard to test, hard to import, and messy in Git. Use them for exploration — scripts for production.

---

**Section summary:** Notebooks are for exploration and reporting. Scripts are for repeatable, importable, production-ready code. Most real work starts in a notebook and ends in a `.py` file.

---

## Live Exercise — Do Together (20 min)

```bash
# 1. Create a project
cd ~/tds/bootcamp
uv init weather-tool
cd weather-tool

# 2. Add a dependency
uv add requests

# 3. Open in VS Code
code .

# 4. Create main.py with this content (type it or paste it)
```

```python
# main.py
import requests

def get_joke():
    url = "https://official-joke-api.appspot.com/random_joke"
    response = requests.get(url)
    data = response.json()
    print(f"Setup: {data['setup']}")
    print(f"Punchline: {data['punchline']}")

if __name__ == "__main__":
    get_joke()
```

```bash
# 5. Run it
uv run main.py

# 6. Check what uv created
cat pyproject.toml
ls -la    # Notice the .venv folder
```

**Discuss:** What would happen if someone cloned your project and ran `uv sync`? What files do they need to recreate the environment?

---

**End of Day 3**

> Remind students: fill in the post-session checklist and push `day3.md` to their GitHub repo.
