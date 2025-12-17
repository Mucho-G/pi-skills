---
name: subagent
description: Spawn a sub-agent to handle tasks independently. Use for delegating subtasks, parallelizing work, or running long analysis in the background.
---

# Subagent Skill

Spawn a sub-agent to handle tasks independently.

## BEFORE SPAWNING - MANDATORY CHECKLIST

**STOP. Do not spawn until you have resolved each item:**

1. **Task type: Read-only or Read-write?**
   - Read-only (analysis, planning, review): `--tools read,grep,find,ls`
   - Read-write (implementation, fixes): `--tools read,bash,edit,write` (default)
   - **bash can execute ANY command. It is NOT read-only. Do NOT use bash for read-only tasks.**

2. **Ask user if not specified:**
   - Model: default or specific? (use `pi --list-models [search]` to find)
   - Thinking: off, minimal, low, medium, high, xhigh?
   - Execution: sync (wait) or async (background)?
   - View result: show subagent session when done?

**Do NOT assume defaults. Ask first.**

---

## Usage

### Synchronous (wait for result)
```bash
pi -p --session /tmp/taskname.jsonl "your prompt here"
```

### Asynchronous (background task)
```bash
# Start in tmux
tmux new-session -d -s taskname 'pi -p --session /tmp/taskname.jsonl "your prompt here" > /tmp/taskname-result.txt 2>&1'

# Check if running
tmux has-session -t taskname 2>/dev/null && echo "running" || echo "done"

# Read result when done
cat /tmp/taskname-result.txt
```

## Flags
- `-p` / `--print`: Non-interactive mode, outputs final response only
- `--session <file>`: Save session to specified JSONL file
- `--provider <name>`: Provider name (e.g., `anthropic`, `openai`, `google`)
- `--model <id>`: Model ID (e.g., `claude-sonnet-4-5`, `gpt-4o`)
- `--thinking <level>`: Set thinking level: `off`, `minimal`, `low`, `medium`, `high`, `xhigh`
- `--tools <tools>`: Comma-separated list of tools (default: `read,bash,edit,write`)
- `--export <file>`: Export session JSONL to HTML

## Selecting a model
```bash
pi --list-models [search]   # Lists available models, optional fuzzy search
pi --list-models sonnet     # Find sonnet models
pi --list-models gpt        # Find GPT models
```
Then pass both `--provider` and `--model`:
```bash
pi -p --session /tmp/task.jsonl --provider anthropic --model claude-sonnet-4-5 "prompt"
```

## Viewing subagent session
Export and open the session:
```bash
pi --export /tmp/taskname.jsonl /tmp/taskname.html && open /tmp/taskname.html
```
