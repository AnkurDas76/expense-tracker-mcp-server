# Expense Tracker MCP Server

A lightweight [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server for tracking personal expenses. Built with [FastMCP](https://github.com/jlowin/fastmcp) and SQLite, it exposes a set of tools an LLM client (like Claude) can call to log, browse, and analyze your spending — no external services required.

## Features

- Local SQLite storage (`expenses.db`) — your data stays on your machine
- Simple add/list tools for logging expenses
- Analytics tools for category summaries, monthly breakdowns, trends, and top spenders
- CSV export for use in spreadsheets or other tools
- Editable categories via a plain JSON file, exposed as an MCP resource

## Requirements

- Python >= 3.12
- [uv](https://docs.astral.sh/uv/) (recommended) or pip

## Installation

```bash
# clone the repo
git clone https://github.com/<your-username>/expense-tracker-mcp-server.git
cd expense-tracker-mcp-server

# install dependencies
uv sync
```

## Running the server

```bash
uv run main.py
```

The server communicates over stdio, so it's meant to be launched by an MCP client (e.g. Claude Desktop, Claude Code) rather than run standalone for interactive use.

### Example MCP client config

```json
{
  "mcpServers": {
    "expense-tracker": {
      "command": "uv",
      "args": ["run", "--directory", "/path/to/expense_tracker_mcp_server", "main.py"]
    }
  }
}
```

## Tools

| Tool | Description |
|---|---|
| `add_expense(date, amount, category, subcategory="", note="")` | Add a new expense entry |
| `list_expenses(start_date, end_date)` | List expenses within an inclusive date range |
| `summarize(start_date, end_date, category=None)` | Sum expenses by category within a date range |
| `monthly_summary(year, month)` | Category breakdown and grand total for a given month |
| `top_categories(start_date, end_date, n=5)` | Top N spending categories within a date range |
| `trend(category, start_date, end_date)` | Month-by-month totals for a single category |
| `export_csv(start_date, end_date, file_path=None)` | Export a date range of expenses to a CSV file |

Dates are expected in `YYYY-MM-DD` format.

## Resources

| Resource | Description |
|---|---|
| `expense://categories` | Serves the contents of `categories.json` (read fresh on every request, so you can edit it without restarting the server) |

## Project structure

```
expense_tracker_mcp_server/
├── main.py            # server entrypoint and tool definitions
├── categories.json     # editable list of expense categories
├── expenses.db          # SQLite database (created on first run)
├── pyproject.toml
└── README.md
```


