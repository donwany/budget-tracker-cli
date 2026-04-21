# 💰 Budget Tracker CLI

A command-line personal finance tracker built with Python and `argparse`, managed with `uv`.

---

## Setup

### 1. Install `uv`

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### 2. Install the project

```bash
uv pip install -e .
```

---

## Usage

Run any command with `uv run budget <subcommand>` or just `budget <subcommand>` after installing.

### Add a transaction

```bash
uv run budget add --amount 45.50 --category food --desc "Lunch at cafe" --type expense
uv run budget add --amount 3000 --category salary --desc "Monthly salary" --type income
uv run budget add --amount 85.20 --category food --desc "Groceries" --type expense --date 2026-04-10
```

### List transactions

```bash
uv run budget list
uv run budget list --month 2026-04
uv run budget list --category food
uv run budget list --type expense
```

### View summary report

```bash
uv run budget summary
uv run budget summary --month 2026-04
```

### Set a budget limit

```bash
uv run budget set-limit --category food --limit 300
uv run budget set-limit --category rent --limit 1500
```

### Export to CSV

```bash
uv run budget export --output report.csv
uv run budget export --month 2026-04 --output april.csv
```

### Delete a transaction (Bonus)

```bash
uv run budget delete --id 3
```

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

## Data Storage

All data is persisted in `data/transactions.json`. The file is auto-created on first run. It contains three keys: `transactions`, `budget_limits`, and `meta`.
