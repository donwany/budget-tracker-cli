# 💰 Personal Budget Tracker CLI — Python Assignment

## Overview

Build a command-line application using Python's `argparse` module that allows a user to **track their personal income and expenses**. This project mirrors real-world CLI tools like the AWS CLI and GitHub CLI, giving you hands-on experience with professional software patterns.

---

## 🎯 Learning Objectives

By completing this project, you will:

- Design and implement a multi-subcommand CLI using `argparse`
- Persist data between program runs using JSON (`transactions.json`)
- Apply modular code organization with a proper project structure
- Handle user input validation and error messages gracefully
- Work with Python's `datetime`, `csv`, and `json` modules
- Manage a Python project professionally using `uv`

---

## 📋 Project Requirements

### Required Subcommands (Core — All Must Be Implemented)

#### 1. `add` — Add a Transaction

```bash
uv run budget.py add --amount 45.50 --category food --desc "Lunch at cafe" --type expense
uv run budget.py add --amount 3000 --category salary --desc "Monthly salary" --type income
```

**Arguments:**

| Argument | Required | Description |
|---|---|---|
| `--amount` | ✅ | Transaction amount (must be > 0) |
| `--category` | ✅ | Category (e.g., food, rent, salary) |
| `--desc` | ✅ | Short description of the transaction |
| `--type` | ✅ | Either `income` or `expense` |
| `--date` | ❌ | Date in `YYYY-MM-DD` format (defaults to today) |

---

#### 2. `list` — View Transactions

```bash
uv run budget.py list
uv run budget.py list --month 2026-04
uv run budget.py list --category food
uv run budget.py list --type expense
```

**Arguments:**

| Argument | Required | Description |
|---|---|---|
| `--month` | ❌ | Filter by month (`YYYY-MM`) |
| `--category` | ❌ | Filter by category |
| `--type` | ❌ | Filter by `income` or `expense` |

---

#### 3. `summary` — Monthly Spending Report

```bash
uv run budget.py summary
uv run budget.py summary --month 2026-04
```

Output should display:
- Total income
- Total expenses
- Net balance
- Breakdown of spending per category
- A warning if any category exceeds its budget limit

---

#### 4. `export` — Export to CSV

```bash
uv run budget.py export --output april_report.csv
uv run budget.py export --month 2026-04 --output april_report.csv
```

**Arguments:**

| Argument | Required | Description |
|---|---|---|
| `--output` | ✅ | Output filename (must end in `.csv`) |
| `--month` | ❌ | Filter export by month |

---

#### 5. `set-limit` — Set a Budget Limit per Category

```bash
uv run budget.py set-limit --category food --limit 300
uv run budget.py set-limit --category rent --limit 1500
```

When running `summary`, warn the user if spending in a category exceeds its set limit.

---

## 🗂️ Required Project Structure

```
budget_tracker/
│
├── pyproject.toml         # uv project config — dependencies and metadata
├── .python-version        # Pinned Python version (auto-created by uv)
├── uv.lock                # Locked dependency versions (auto-created by uv)
├── src/
│   └── budget_tracker/
│       ├── __init__.py
│       ├── __main__.py        # Entry point — argparse setup and subcommand routing
│       ├── commands/
│       │   ├── __init__.py
│       │   ├── add.py             # Logic for the add subcommand
│       │   ├── list_transactions.py  # Logic for the list subcommand
│       │   ├── summary.py         # Logic for the summary subcommand
│       │   ├── export.py          # Logic for the export subcommand
│       │   └── set_limit.py       # Logic for the set-limit subcommand
│       └── storage.py             # All file I/O — read/write transactions and limits
├── data/
│   └── transactions.json  # Persisted transaction data (auto-created)
└── README.md              # How to install and use your app
```

> ⚠️ **Important:** All data reading and writing must go through `storage.py`. No command file should directly open or write to files.

---

## ✅ Grading Rubric

| Category | Points | Criteria |
|---|---|---|
| **`add` subcommand** | 15 | Saves transaction, validates amount > 0, validates type is income/expense |
| **`list` subcommand** | 15 | Displays transactions, filters work correctly |
| **`summary` subcommand** | 20 | Correct totals, per-category breakdown, budget warnings |
| **`export` subcommand** | 10 | Valid CSV output, correct filtering |
| **`set-limit` subcommand** | 10 | Limits saved and applied in summary |
| **Data persistence** | 10 | Data survives between runs (`JSON`) |
| **Code structure** | 10 | Follows required folder/module structure |
| **Error handling** | 5 | Handles invalid inputs gracefully with clear messages |
| **README** | 5 | Clear setup and usage instructions |
| **Bonus features** | +10 | See bonus section below |

**Total: 100 points (+10 bonus)**

---

## 🏆 Bonus Challenges (+10 points max)

Choose any of the following for extra credit:

- **Recurring transactions** — Add a `--recurring` flag to `add`; these auto-apply each month
- **Terminal charts** — Use the `rich` library to display a bar chart of spending by category in `summary`
- **Multi-user profiles** — Add a `--user` flag to all commands to support separate data per user
- **Currency support** — Add a `--currency` flag and convert amounts using a free exchange rate API
- **Delete command** — Add a `delete` subcommand to remove a transaction by ID

---

## 🚫 Common Mistakes to Avoid

- Do **not** hardcode file paths — use `os.path` to build them relative to the script
- Do **not** let the program crash with a traceback on bad input — catch errors and print helpful messages
- Do **not** put data logic inside `budget.py` — keep it modular
- Do **not** forget to handle the case where `data/transactions.json` doesn't exist yet

---

## 🛠️ Project Setup with `uv`

This project must be set up and run using [`uv`](https://docs.astral.sh/uv/), a modern Python package manager.

### 1. Install `uv` (if not already installed)

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### 2. Initialize the project

```bash
uv init --package budget_tracker --python 3.11/3.12/3.13/3.14
uv venv
source .venv/bin/activate

cd budget_tracker
```

### 3. Your `pyproject.toml` should look like this

```toml
[project]
name = "budget-tracker"
version = "0.1.0"
description = "A personal budget tracker CLI"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[project.scripts]
budget = "budget_tracker.__main__:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

### 4. Add the bonus `rich` dependency (if doing bonus)

```bash
uv add rich
```

### 5. Run the app

```bash
# Using uv run (recommended — no activation needed)
uv run budget add --amount 45.50 --category food --desc "Lunch" --type expense

# Or after installing as a script
uv pip install -e .
budget add --amount 45.50 --category food --desc "Lunch" --type expense
```

> ✅ **Never use `pip install` directly.** All dependency management must go through `uv`.

---

## 📦 Allowed Libraries

You may use only Python's **standard library** plus these approved packages:

- `argparse`, `json`, `csv`, `datetime`, `os` *(standard library — no install needed)*
- `rich` *(bonus only — add with `uv add rich`)*

No other third-party packages are allowed unless approved in advance.

---

## 🗓️ Submission

| Item | Details |
|---|---|
| **Due date** | *May 16, 2026* |
| **Submit via** | *GITHUB AND PYPI* |

Make sure your project includes `pyproject.toml` and `uv.lock` so your project can be reproduced exactly.

Include a `README.md` with:
1. How to set up and run the app using `uv`
2. At least 5 example `uv run` commands
3. Any bonus features you implemented

---

## 💡 Tips for Success

- Start with `storage.py` — get data saving and loading working before building commands
- Test each subcommand independently before wiring them all together
- Use `argparse`'s built-in `type=float` and `choices=[...]` to handle validation automatically
- Run `uv run budget --help` and `uv run budget add --help` frequently to check your CLI looks right
- Commit `uv.lock` to version control — it ensures your project runs the same on any machine

---

*Good luck! A well-built CLI tool is something you can genuinely add to your portfolio.* 🚀
