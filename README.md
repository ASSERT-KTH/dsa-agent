# Data science agent

A data science agent conducts its session as an append-only computational
notebook. It uses one persistent Python REPL for computation, file operations,
and program execution. Every session is maintained as a valid Jupyter notebook:
questions and answers become Markdown cells, and Python calls become code cells
with their outputs.

The notebook is a record of the investigation. Executed cells are never
rewritten by the agent; subsequent work is appended with new outputs.

## Requirements

- Python 3.11 or newer
- a Kimi Coding Plan API key in the system keyring:

```bash
keyring set login2 kimi_api_key
```

The agent uses `agentknit` and its `kimi-coding` inference configuration.

## Run

```bash
uv venv
uv pip install -e '.[dev]'
.venv/bin/dsa-agent "Explore the dataset in data.csv"
```

Interactive mode starts when no task is supplied:

```bash
.venv/bin/dsa-agent
```

Useful options:

```bash
.venv/bin/dsa-agent --session <id>       # resume a session
.venv/bin/dsa-agent --non-interactive   # disable questions
.venv/bin/dsa-agent --notebook          # print the notebook path
.venv/bin/dsa-agent --notebook out.ipynb "Analyze data.csv"
```

New sessions store the notebook beside their JSONL transcript. Resumed sessions
use `<session_id>.ipynb` beside agentknit's snapshot. Python execution is
unsandboxed and inherits the user's permissions.

## Development

```bash
.venv/bin/pytest
.venv/bin/ruff check .
```

MIT license.
