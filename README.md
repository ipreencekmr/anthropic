# Anthropic — Claude Exercises & MCP Chat

A hands-on collection for learning to work with Anthropic's Claude models. It has two parts:

1. **Colab notebooks** — exercise-style notebooks covering the Claude API, prompt engineering, evaluation, tool use, and RAG/agentic search.
2. **`mcp_cli/`** — a runnable command-line chat app built on the Model Context Protocol (MCP).

## Repository Structure

```
anthropic/
├── Claude_With_API.ipynb        # Basics of calling the Claude API
├── Features_of_Claude.ipynb     # Claude features (vision, PDFs, streaming, etc.)
├── Prompt_Engineering.ipynb     # Prompt design and optimization
├── Prompt_Evaluation.ipynb      # Evaluating and scoring prompt outputs
├── Tools_Usage.ipynb            # Tool use / function calling
├── RAG_N_Agentic_Search.ipynb   # Retrieval-augmented generation & agentic search
├── mcp_cli/                     # CLI chat app powered by MCP (see mcp_cli/README.md)
├── images/                      # Sample images used in notebooks
├── pdf/                         # Sample PDFs used in notebooks
├── files/                       # Sample data files (e.g. streaming.csv)
└── report.md                    # Long-form sample document for demos
```

## Part 1 — Notebooks

| Notebook | Description |
|---|---|
| `Claude_With_API.ipynb` | Authenticating and making calls to the Claude API. |
| `Features_of_Claude.ipynb` | Claude capabilities such as vision, PDF input, and streaming. |
| `Prompt_Engineering.ipynb` | Writing clear, effective prompts. |
| `Prompt_Evaluation.ipynb` | Systematically evaluating prompt/response quality. |
| `Tools_Usage.ipynb` | Claude's tool use / function-calling. |
| `RAG_N_Agentic_Search.ipynb` | Retrieval-augmented generation and agentic search. |

### Running the notebooks

1. Open any `.ipynb` on GitHub and choose **Open in Colab**.
2. Set your API key:
   ```python
   import os
   os.environ["ANTHROPIC_API_KEY"] = "your_api_key_here"
   ```
3. Run the cells top to bottom. Notebooks are self-contained and can be run independently. If a notebook reads from `images/`, `pdf/`, or `files/`, make those files available in your Colab environment first.

## Part 2 — MCP Chat CLI (`mcp_cli/`)

An interactive terminal chat client that talks to Claude and an MCP server. It supports `@document` mentions to inject context and `/command` prompts served by the MCP server.

```bash
cd mcp_cli
uv venv && source .venv/bin/activate
uv pip install -e .
uv run main.py
```

Requires a `mcp_cli/.env` with `ANTHROPIC_API_KEY` and `CLAUDE_MODEL` set. See [`mcp_cli/README.md`](mcp_cli/README.md) for full setup and usage.

## Prerequisites

- An [Anthropic API key](https://console.anthropic.com/)
- A Google account for the notebooks; Python 3.10+ for the CLI app
