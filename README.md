# 🐛 Bug Whisperer — Find Mistakes in Your Python Code

> **An AI-agent skill plus a CLI/pytest plugin that find your coding mistakes,
> explain them in plain English, and guide you to a fix — safely.**

**Bug Whisperer** is a Python package that helps you find, understand, and fix
mistakes in your code. It works two ways:

1. **As an AI-agent skill** — give it to any AI coding agent (Claude Code, Cursor,
   Codex, ChatGPT Codex, Devin, or any agent that can read files, run commands,
   and edit files). The skill tells the agent exactly how to **investigate** failing tests
   and review code, step by step, with evidence for every claim — **the agent
   does not edit your files without your approval.**
2. **As a command-line tool and pytest plugin** — install it with `pip install .`
   and get friendly boxed error reports instead of raw tracebacks, in any terminal
   or IDE, with zero configuration.

It is **IDE- and platform-agnostic**. It does not depend on any specific editor,
AI provider, or cloud service. The core engine has zero third-party dependencies.

---

## 📖 Table of Contents

1. [What is this?](#-what-is-this)
2. [What does it do?](#-what-does-it-do)
3. [For Python learners](#for-python-learners)
4. [What do I need before I start?](#-what-do-i-need-before-i-start)
5. [Step 1 — Install the tool](#-step-1--install-the-tool)
6. [Step 2 — Use it in your Terminal](#-step-2--use-it-in-your-terminal)
7. [Step 3 — Use it with pytest](#-step-3--use-it-with-pytest)
8. [Step 4 — Add the skill to your AI agent](#-step-4--add-the-skill-to-your-ai-agent)
9. [How the AI agent skill works](#how-the-ai-agent-skill-works)
10. [Example: What the output looks like](#-example-what-the-output-looks-like)
11. [Demo files — learn by finding mistakes](#demo-files--learn-by-finding-mistakes)
12. [Troubleshooting](#-troubleshooting)
13. [Project File Map](#-project-file-map)
14. [Is it safe to use on my real code?](#is-it-safe-to-use-on-my-real-code)
15. [About this project](#about-this-project)

---

## 🤔 What is this?

**Bug Whisperer** is a tool that reads your Python code and tells you exactly
**what went wrong**, **where it went wrong**, and **how to fix it** — in simple,
friendly language.

Think of it like a helpful teacher looking over your shoulder while you code.
When something breaks, instead of seeing a scary red error message, you see a
neat box that says:

- 📍 **WHERE** the mistake is (exact file and line number)
- ❌ **WHAT WENT WRONG** (explained in plain English)
- 💡 **SUGGESTIONS** (step-by-step tips to fix it)

---

## ✨ What does it do?

It has **two main modes**:

### Mode 1 — Triage Failing Tests 🔴→🟢
When your tests are failing (showing red errors), Bug Whisperer helps an agent:
1. Look at every failing test at the same time (in parallel)
2. Figure out the root cause of each failure, with file:line evidence
3. Propose the smallest possible fix for each one
4. Apply one fix at a time and verify it — rolling back anything that fails,
   and stopping for human review when it cannot auto-repair

### Mode 2 — Review Your Code 🔍
When you just wrote some code and want a second opinion, Bug Whisperer:
1. Checks for logic errors and edge cases
2. Checks for security issues
3. Checks for missing error handling
4. Gives you a ranked list of findings with evidence

---

## 🎓 For Python learners

Learning Python? Bug Whisperer is built for you. Here's how to use it to
learn faster:

### The demo files are learning exercises

The `demos/` folder has Python demo files, each with **one intentional mistake**.
Run them and read the box — you'll learn:

| Demo file | What you'll learn |
|-----------|------------------|
| `demos/demo_math.py` | Why you can't take the square root of a negative number |
| `demos/demo_json.py` | Why JSON requires double quotes, not single quotes |
| `demos/demo_requests.py` | Why you always need `https://` in a URL |
| `demos/demo_numpy.py` | Why arrays must have the same shape to add them |
| `demos/demo_pandas.py` | Why column names are case-sensitive in pandas |
| `demos/demo_matplotlib.py` | Why matplotlib rejects invalid color names |
| `demos/demo_sklearn.py` | Why scikit-learn needs 2D arrays, not 1D |
| `demos/demo_sqlalchemy.py` | Why database URLs must follow a specific format |
| `demos/demo_pydantic.py` | Why Pydantic rejects the wrong data type |
| `demos/demo_bs4.py` | Why BeautifulSoup needs a real parser name |
| `demos/demo_openpyxl.py` | Why Excel cell references must be valid |
| `demos/demo_pillow.py` | Why you can't open a file that doesn't exist |
| `demos/demo_scipy.py` | Why some matrices can't be inverted |
| `demos/demo_seaborn.py` | Why seaborn can't plot None data |
| `demos/demo_tqdm.py` | Why you can't iterate over None |

### How to practice

```bash
# Run a demo and see the mistake explained
mistake-box python demos/demo_math.py

# Or check a file for syntax errors without running it
mistake-box --file demos/demo_json.py

# Run all the demos through the interactive menu
python menu.py
```

Each demo shows you:
1. The **wrong code** (with a comment explaining what's wrong)
2. The **error** (parsed and explained in a box)
3. The **correct code** (with a comment showing the fix)

Read the wrong code, predict what error you'll get, then run it and check
your answer against the box.

---

## 📋 What do I need before I start?

You need **2 things** installed on your computer:

| Thing | What it is | Where to get it |
|-------|-----------|-----------------|
| **Python 3.8 or newer** | The programming language this tool uses | [python.org/downloads](https://python.org/downloads) |
| **Git** (optional) | A tool to download code from GitHub | [git-scm.com](https://git-scm.com/downloads) |

> 💡 **Not sure if you have Python?** Open a terminal (Command Prompt on Windows) and type:
> ```
> python --version
> ```
> If you see something like `Python 3.11.0` — you're good! If you see an error, install Python from [python.org/downloads](https://python.org/downloads).

> 💡 **Not sure if you have pip?** Try `python -m pip --version`. If that fails, reinstall Python and make sure to check **"Add Python to PATH"** during installation.

---

## 📦 Step 1 — Install the tool

### Option A — Install from GitHub (recommended)

Open your terminal and run:

```bash
git clone https://github.com/your-username/Bug-Whisperer.git
cd Bug-Whisperer
pip install .
```

> **Note:** Replace `your-username` with your actual GitHub username before running this command.

### Option B — Install from a local copy

If you've already downloaded the project folder, open a terminal in that
folder and run:

```bash
pip install .
```

> This command reads the project and installs everything automatically. It takes about 10–30 seconds.

**What this does:**
- Installs a command called `mistake-box` that you can use anywhere
- Installs a pytest plugin that automatically shows errors in a friendly box every time you run tests

> ✅ If you see `Successfully installed mistake-finder-1.0.0`, installation was successful!

---

## 💻 Step 2 — Use it in your Terminal

You don't need any special IDE to use this tool. Open any terminal and try it.

### Run your code and see errors in a friendly box:

```bash
mistake-box python mycode.py
```

Instead of a raw Python traceback, you'll see a neat box like this:

```
╔══════════════════════════════════════════════════════════════╗
║ MISTAKE FOUND - SyntaxError                                  ║
╠══════════════════════════════════════════════════════════════╣
║ WHERE                                                        ║
║   mycode.py:12                                               ║
║ >>  12 |     if x = 5:                                       ║
║               ^                                              ║
╠══════════════════════════════════════════════════════════════╣
║ WHAT WENT WRONG                                              ║
║   SyntaxError: invalid syntax. Maybe you meant '==' or ':='? ║
╠══════════════════════════════════════════════════════════════╣
║ SUGGESTIONS                                                  ║
║   1. A single `=` assigns; use `==` to compare.              ║
╚══════════════════════════════════════════════════════════════╝
```

### Other ways to use it:

| What you want to do | Command |
|---------------------|---------|
| Run a Python file and catch errors | `mistake-box python mycode.py` |
| Check a file for syntax errors only | `mistake-box --file mycode.py` |
| Parse an existing log/error file | `mistake-box --log ci.log` |
| Run with a plain ASCII box (no Unicode) | `mistake-box --ascii python mycode.py` |
| Show the original error too | `mistake-box --raw python mycode.py` |



### Turn off the box globally (advanced):

```bash
set MISTAKE_BOX=0      # Windows
export MISTAKE_BOX=0   # Mac/Linux
```

---

## 🧪 Step 3 — Use it with pytest

Once installed, the pytest plugin works automatically. Every time you run
`pytest`, any failing tests are shown as friendly boxes.

```bash
# Run your tests — failing ones get boxed automatically
pytest

# Run tests without the boxes
pytest --no-mistake-box

# Run a specific test file
pytest tests/math.test.py
```

The `tests/` folder has pytest test files that cover several popular Python
libraries. Each one has an intentional mistake — run them to see how the
pytest plugin works.

```bash
# Run all tests and see every mistake in a box
pytest

# Run just one library's test
pytest tests/json.test.py
```

---

## 🤖 Step 4 — Add the skill to your AI agent

Bug Whisperer includes an **AI-agent skill** — a set of instructions that tell
any AI coding agent exactly how to find and fix mistakes in your code.

### What is an AI agent skill?

An AI agent skill is a file (usually `SKILL.md`) that you give to your AI
coding agent. When you ask the agent to "fix my failing tests" or "review my
code", the agent reads the skill file and follows its instructions. The skill
tells the agent:

- How to investigate failing tests systematically
- How to trace errors back to their root cause
- How to propose minimal fixes and verify them
- How to review code for common mistake patterns
- What rules to follow (never weaken tests, always provide evidence, etc.)

### How to add the skill to your agent

The skill lives in `.bob/skills/mistake-finder/`. The folder name comes from
the original design, but the skill itself is **IDE-agnostic** — you can use it
with any AI agent.

**Option A — Project-scoped (recommended for learning)**

Keep the skill in your project's `.bob/skills/mistake-finder/` folder. When
you ask your AI agent to work on this project, tell it:

> "Use the mistake-finder skill in `.bob/skills/mistake-finder/SKILL.md` to
> help with this task."

The agent reads the skill and follows it.

**Option B — Global install (works in every project)**

```bash
python install_skill.py
```

This copies the skill to `~/.bob/skills/mistake-finder/` so it's available in
every project you work on.

To check if it worked:
```bash
python install_skill.py --check
```

To remove it later:
```bash
python install_skill.py --remove
```

### What to ask your AI agent

Once the skill is available, talk to your agent naturally:

**If your tests are failing:**
```
Run mistake-finder on the failing tests.
```

**If you just wrote some code and want it reviewed:**
```
Check my code for mistakes.
```

**To review a specific file:**
```
Check src/myfile.py for mistakes.
```

**If you have a log file from a CI/CD pipeline:**
```
Run mistake-finder on artifacts/ci.log.
```

---

## 🧠 How the AI agent skill works

The skill (`SKILL.md`) defines a four-phase workflow that any AI agent can
follow. Here's what happens behind the scenes:

### Phase 0 — Understand the situation

The agent figures out:
- What language and test runner the project uses
- Which tests are failing (from actual test output, not memory)
- Or, for code review, which files to inspect

### Phase 1 — Fan out investigation

The agent splits the work into parallel investigations:

- **For failing tests**: one investigation per failing test (or per group of
  related failures). Each investigation traces from the failing assertion back
  to the line of code that caused it.
- **For code review**: one investigation per mistake class (logic errors,
  error handling, security issues, performance problems, etc.). Each
  investigation checks the code for that specific kind of mistake.

### Phase 2 — Aggregate findings

The agent combines all investigations into one ranked report:
- Most severe findings first (correctness and security issues before style)
- Every finding has a `file:line` pointer to the evidence
- Conflicting findings are surfaced, not silently resolved

### Phase 3 — Fix loop (for failing tests, with rollback)

The agent works through the ranked list, one fix at a time, and **never
edits test files, never weakens assertions, and never skips a failing test**:
1. Apply one minimal fix to source code only
2. Re-run the affected test
3. If it passes → move on
4. If it fails → revert the fix and flag the row for human review
5. Stop when everything is resolved or two attempts on the same row have failed

### Phase 4 — Verify and report

The agent runs the full test suite one more time and produces a final summary:
- What was found
- What was fixed
- What was rolled back (needs human review)
- Recall rate (fixed / total failures)

---

## 🖼️ Example: What the output looks like

Here's a real example. Imagine your test file has this failing test:

```python
def test_add():
    assert add(2, 3) == 5   # but add() returns -1 by mistake
```

Running `pytest` will now show:

```
╔════════════════════════════════════════════════════════════════╗
║ MISTAKE FOUND - AssertionError                                 ║
╠════════════════════════════════════════════════════════════════╣
║ TEST                                                           ║
║   tests/test_calc.py::test_add                                 ║
╠════════════════════════════════════════════════════════════════╣
║ WHERE                                                          ║
║   tests/test_calc.py:5                                         ║
║      4 | def test_add():                                       ║
║ >>   5 |     assert add(2, 3) == 5                             ║
╠════════════════════════════════════════════════════════════════╣
║ WHAT WENT WRONG                                                ║
║   AssertionError: assert -1 == 5  (where -1 = add(2, 3))      ║
╠════════════════════════════════════════════════════════════════╣
║ SUGGESTIONS                                                    ║
║   1. Read the assertion: what value did the code actually      ║
║      produce, and what did the test expect?                    ║
║   2. The function returned -1 but 5 was expected. Check        ║
║      the logic inside add().                                   ║
╚════════════════════════════════════════════════════════════════╝
```

Much friendlier than a raw Python traceback!

---

## 📂 Demo files — learn by finding mistakes

The `demos/` folder has Python demo files, each with **one intentional mistake**
and the correct version commented out. Use them to:

1. **Learn what common mistakes look like** — each demo covers a different
   type of error you'll encounter in real Python code.
2. **Practice reading tracebacks** — run a demo, see the raw error, then
   compare it to the boxed version.
3. **Test the tool** — if you're developing Bug Whisperer itself, the demos
   are a ready-made test suite.

### How to use a demo

```bash
# See the mistake explained
mistake-box python demos/demo_math.py

# Check for syntax errors only (doesn't run the code)
mistake-box --file demos/demo_json.py

# Open the file in your editor and compare the wrong/correct versions
code demos/demo_requests.py
```

### The test files

The `tests/` folder has matching test files for some demos. These are
**pytest test files** with intentional failures — they're designed to show
how the pytest plugin boxes test failures. Not every demo has a matching test file.

```bash
# Run one test file and see the boxed failure
pytest tests/math.test.py

# Run all tests and see all failures boxed
pytest
```

---

## 🆘 Troubleshooting

### ❓ "python is not recognized as a command"
**Fix:** Python is not installed or not added to PATH.
- Reinstall Python from [python.org](https://python.org/downloads) and make sure to check **"Add Python to PATH"** during installation.
- Restart your terminal after installing.

---

### ❓ "pip is not recognized as a command"
**Fix:** Try using `pip3` instead of `pip`:
```bash
pip3 install .
```
Or try:
```bash
python -m pip install .
```

---

### ❓ "mistake-box is not recognized as a command"
**Fix:** Run `pip install .` again from inside the `Bug-Whisperer` folder. Make sure you see `Successfully installed mistake-finder-...` at the end.

If you're using a virtual environment, make sure it's activated when you install and when you run `mistake-box`.

---

### ❓ Tests are not showing the friendly box
**Fix:** Make sure you installed the tool first with `pip install .` in the `Bug-Whisperer` folder. Also check that you're using the same Python/pip that runs your tests.

To verify the plugin is installed:
```bash
pytest --help | grep mistake
```
You should see `--mistake-box` and `--no-mistake-box` options.

---

### ❓ The box shows weird characters instead of box-drawing lines
**Fix:** Your terminal doesn't support Unicode box-drawing characters. Use the ASCII box instead:
```bash
mistake-box --ascii python mycode.py
```

Or set it as the default:
```bash
export MISTAKE_BOX_ASCII=1   # Mac/Linux
set MISTAKE_BOX_ASCII=1      # Windows
```

---

### ❓ "git is not recognized as a command"
**Fix:** Git is not installed. Download and install from [git-scm.com](https://git-scm.com/downloads), then restart your terminal.

> **Note:** Git is only needed to clone the project from GitHub. You don't need Git to use the tool after it's installed.

---

### ❓ The AI agent doesn't follow the skill
**Fix:** Make sure the agent knows the skill exists. Try:

> "Read the file `.bob/skills/mistake-finder/SKILL.md` and follow its instructions for this task."

Different agents load skills differently. Check your agent's documentation for how to add custom skills or instructions.

---

## 🗂️ Project File Map

Here's what every file and folder in this project does:

```
Bug-Whisperer/
│
├── .bob/skills/mistake-finder/   ← The AI-agent skill (works with any agent)
│   ├── SKILL.md                   ← The skill instructions (the "brain")
│   ├── review-checklist.md        ← Mistake classes for code review mode
│   ├── report-template.md         ← How reports are formatted
│   ├── box.py                     ← Core engine: parses errors and draws the box
│   ├── mistake_box.py             ← The `mistake-box` command for terminals
│   ├── pytest_plugin.py           ← Auto-shows boxes when pytest tests fail
│   └── __init__.py                ← Makes the folder work as a Python package
│
├── demos/                         ← 14 demo files with intentional mistakes
│   (one per popular Python library — great for learning)
│
├── tests/                         ← 15 test files with intentional failures
│   (one per popular Python library — shows pytest plugin in action)
│
├── mycode.py                      ← Template: paste your own code here to check it
│
├── find_mistakes.py               ← Smart runner — detects which file to analyse
├── smart_run.py                   ← Auto-detects which library a file uses
├── menu.py                        ← Interactive menu to pick what to run
├── install_skill.py               ← Installs the skill globally (~/.bob/skills/)
│
├── pyproject.toml                 ← Package config (used by pip install)
├── README.md                      ← This file!
├── LICENSE                        ← MIT license
├── .env.example                   ← Template for configuration (if needed)
├── .env                           ← Your local configuration (never committed)
│
├── run.bat                        ← Run mycode.py and catch mistakes
├── check.bat                      ← Syntax-check a file (no run)
├── test.bat                       ← Open the interactive menu
├── run.ps1, check.ps1, test.ps1  ← Same launchers for PowerShell
│
├── index.html                     ← Project landing page
│
├── .gitignore                     ← Files that Git should ignore
└── .venv/                         ← Python virtual environment (if used)
```

---

## 🛡️ Is it safe to use on my real code?

For the **core tool** (CLI + pytest plugin), yes — it runs entirely on your
computer, has no API keys, makes no network calls, and never touches your files
except to read them.

For the **AI-agent skill**, safety depends on how you use it. The skill
(`SKILL.md`) is written so a well-behaved agent:

- **Investigates read-only** — it reads your files to find mistakes; it does not
  edit them while investigating.
- **Never weakens tests** — it does not skip, delete, or soften failing tests.
- **Asks before changing code** — the fix loop proposes one minimal source-code
  fix at a time, verifies it, and rolls back anything that fails; the agent
  should stop for human review when it cannot auto-repair.
- **Every claim has evidence** — a finding without a `file:line` pointer is not
  reported.

Those are guardrails written into the skill instructions. They work when the
agent follows them. They are **not** a guarantee the agent will obey them —
any AI agent can ignore instructions. Treat the skill the same way you would
treat any AI coding assistant that can edit files: use it on copies or version-
controlled code first, review every change it proposes, and do not let it run
unattended on code you cannot afford to lose.

**Voice mode is an exception** — if you use `voice_agent.py`, your voice is sent
to IBM Watson for transcription and a network call is made. If you do not use
voice mode, the tool makes no network calls.

---

## 📋 About this project

**Bug Whisperer** is a Python package that makes debugging easier and more
approachable — especially for people learning Python.

### What makes it different

- **No dependencies** — the core engine is pure Python, no third-party packages
- **Works everywhere** — any IDE, any terminal, any OS, any AI agent
- **Two tools in one** — a CLI/pytest plugin for instant feedback, plus an AI-agent skill for deeper analysis
- **Learner-friendly** — the demo files are ready-made exercises for practicing error-reading
- **Open source** — use it, study it, modify it, share it

### Package info

| Field | Value |
|-------|-------|
| Package name | `mistake-finder` |
| CLI command | `mistake-box` |
| Version | 1.0.0 |
| Python requirement | 3.8+ |
| Third-party dependencies | None (core engine) |
| License | MIT |

### Built for learning and productivity

Whether you're learning Python for the first time, teaching others, or building
real projects, Bug Whisperer helps you understand your mistakes faster so you
can fix them and move on.

---

*Happy coding — and may your bugs be few! 🐞*
