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
- an OpenAI-compatible Chat Completions API server

```bash
export DSA_MODEL="your-model"
export DSA_BASE_URL="http://localhost:8000/v1"
export DSA_API_KEY="your-key"
```

The agent uses `agentknit` to talk to the configured server. It has no
provider-specific configuration.

## Run

```bash
uv venv
uv pip install -e '.[dev]'
.venv/bin/dsa "Explore the dataset in data.csv"
```

Interactive mode starts when no task is supplied:

```bash
.venv/bin/dsa
```

Useful options:

```bash
.venv/bin/dsa --session <id>       # resume a session
.venv/bin/dsa --non-interactive   # disable questions
.venv/bin/dsa --print-notebook    # print the notebook path
.venv/bin/dsa --notebook out.ipynb "Analyze data.csv"
```

The optional `DSA_CONTEXT_WINDOW` sets the model context size (default 131072),
and `DSA_CACHE_TTL_SECONDS` sets the prompt-cache lifetime (default 300).
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
