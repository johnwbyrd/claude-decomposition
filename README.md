# claude-rlm

A Claude Code plugin for systematic context decomposition. Inspired by the
[RLM framework](https://github.com/alexzhang13/rlm) from MIT OASYS lab
([paper](https://arxiv.org/abs/2512.24601),
[blog](https://alexzhang13.github.io/blog/2025/rlm/)).

## What This Does

Teaches Claude Code to systematically decompose large contexts and complex
problems instead of attempting to process everything in a single pass. The
skill provides:

- A context assessment protocol (measure before decomposing)
- Three decomposition primitives mapped to Claude Code's Task tool
- A chunking utility for splitting large files
- A persistent REPL server for stateful computation across Bash calls
- Worked examples for common scenarios

## Installation

```bash
claude --plugin-dir ~/git/claude-rlm/
```

Or symlink into your plugins directory for persistent use.

## Usage

The skill activates automatically when Claude Code encounters large-context
analysis tasks. Trigger phrases include:

- "Analyze this large file"
- "Process multiple files"
- "Chunk and analyze"
- "Recursive analysis"
- Any task involving input larger than ~50KB

## Bundled Scripts

### chunk_text.py

Deterministic text chunking:

```bash
python3 skills/rlm-decomposition/scripts/chunk_text.py info large_file.txt
python3 skills/rlm-decomposition/scripts/chunk_text.py chunk large_file.txt --size 80000
python3 skills/rlm-decomposition/scripts/chunk_text.py boundaries source.py
```

### repl_server.py / repl_client.py

Persistent Python REPL over Unix domain socket. Variables, imports, and
function definitions survive across Bash calls:

```bash
# Start the server
python3 skills/rlm-decomposition/scripts/repl_server.py /tmp/repl.sock &

# Execute code (state persists between calls)
python3 skills/rlm-decomposition/scripts/repl_client.py /tmp/repl.sock 'x = 42'
python3 skills/rlm-decomposition/scripts/repl_client.py /tmp/repl.sock 'print(x + 1)'

# Inspect variables
python3 skills/rlm-decomposition/scripts/repl_client.py /tmp/repl.sock --vars

# Shut down
python3 scripts/repl_client.py /tmp/repl.sock --shutdown
```

All scripts are pure Python 3, no external dependencies.
