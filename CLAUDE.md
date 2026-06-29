# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository layout

Two unrelated bodies of work share this repo:

1. **Top-level Jupyter notebooks** (`*.ipynb`) — self-contained, exercise-style Colab notebooks for learning the Anthropic API (basics, prompt engineering, prompt evaluation, tool use, RAG/agentic search). They are designed to run top-to-bottom in Google Colab and read shared assets from `images/`, `pdf/`, `files/`, and `report.md`. Each notebook expects `ANTHROPIC_API_KEY` set as an env var / Colab secret.
2. **`mcp_cli/`** — a runnable Python application: a CLI chat client wired to an MCP server. This is the real code project; almost all development work happens here.

## mcp_cli — commands

All commands run from `mcp_cli/`. Requires `mcp_cli/.env` with `ANTHROPIC_API_KEY` and `CLAUDE_MODEL` set (both are asserted non-empty at startup in `main.py`).

```bash
cd mcp_cli
uv venv && source .venv/bin/activate
uv pip install -e .
uv run main.py                 # start the CLI chat
```

- `USE_UV=1` makes the app launch the bundled MCP server with `uv run mcp_server.py` instead of `python mcp_server.py`.
- Extra MCP servers can be attached by passing their script paths as CLI args: `uv run main.py path/to/other_server.py` (each is launched via `uv run`).
- There are **no lint, type-check, or test commands** configured in this project.

## mcp_cli — architecture

The app is an agentic tool-use loop on top of MCP servers spoken over stdio.

- **`main.py`** — entrypoint. Loads `.env`, constructs `Claude`, and uses an `AsyncExitStack` to spin up MCP clients. The first/default client (`doc_client`) always runs the bundled `mcp_server.py`; additional servers from argv are added to the `clients` dict. Wires everything into `CliChat` → `CliApp`.
- **`mcp_client.py` (`MCPClient`)** — async context manager wrapping one `mcp.ClientSession` over a stdio subprocess. Exposes `list_tools`, `call_tool`, `list_prompts`, `get_prompt`, `read_resource`. The commented-out stub methods near the top are intentionally superseded by the real implementations below them.
- **`mcp_server.py`** — the bundled MCP server (`FastMCP`, name `DocumentMCP`). Documents live in an in-memory `docs` dict. Exposes tools (`read_doc_contents`, `edit_document`), resources (`docs://documents`, `docs://documents/{doc_id}`), and a prompt (`format`). **To add a document, edit the `docs` dict here.** New tools/resources/prompts are added with the `@mcp.tool` / `@mcp.resource` / `@mcp.prompt` decorators.
- **`core/claude.py` (`Claude`)** — thin Anthropic SDK wrapper. `chat()` builds the params (max_tokens 8000, optional `thinking`, `tools`, `system`) and calls `messages.create`. Message-list helpers normalize between SDK `Message` objects and plain dicts.
- **`core/chat.py` (`Chat`)** — the core agent loop. `run()` appends the query, then repeatedly calls Claude; while `stop_reason == "tool_use"` it executes the requested tools via `ToolManager` and feeds results back as a user message, looping until a plain text response ends it.
- **`core/cli_chat.py` (`CliChat`)** — subclass of `Chat` that adds CLI-specific query preprocessing:
  - `@docname` mentions → resource content is fetched and injected into the prompt as `<document>` context (`_extract_resources`).
  - `/command arg` → resolved to an MCP prompt via `get_prompt` and converted to message params (`_process_command`); these conversions are handled by `convert_prompt_messages_to_message_params`.
- **`core/cli.py` (`CliApp`)** — the `prompt_toolkit` terminal UI: completion/auto-suggest for `/commands` (from MCP prompts) and `@resources` (from MCP doc ids), key bindings, and the main input loop.
- **`core/tools.py` (`ToolManager`)** — aggregates tools across all clients (`get_all_tools`), routes a tool call to the first client that advertises it (`_find_client_with_tool`), and packages results into Anthropic `tool_result` blocks.

### Key flow

A user line entered in `CliApp.run` → `CliChat.run` (preprocess `@`/`/`) → `Chat.run` agent loop → `Claude.chat` ↔ `ToolManager.execute_tool_requests` → MCP servers. Adding a capability the model can use means adding it to an MCP server (`mcp_server.py` or an external one), not changing the loop.
