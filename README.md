# Anthropic — Colab Notebook Exercises

A collection of Google Colab notebooks exploring different ways to work with Anthropic's Claude models — from basic API usage to prompt engineering, evaluation, tool use, and retrieval-augmented (agentic) search.

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Notebooks](#notebooks)
- [Supporting Folders](#supporting-folders)
- [Getting Started](#getting-started)
- [Usage](#usage)

## Overview

This repository is a hands-on, exercise-style collection for learning and experimenting with Claude via the Anthropic API. Each notebook is self-contained and focuses on a specific theme, while shared sample assets (images, PDFs, a sample report) are used across notebooks for demonstrations.

## Repository Structure

```
anthropic/
├── images/                      # Sample images used in notebook exercises
├── pdf/                         # Sample PDFs used in notebook exercises
├── Claude_With_API.ipynb        # Basics of calling the Claude API
├── Prompt_Engineering.ipynb     # Prompt design and optimization techniques
├── Prompt_Evaluation.ipynb      # Evaluating and scoring prompt outputs
├── RAG_N_Agentic_Search.ipynb   # Retrieval-augmented generation & agentic search
├── Tools_Usage.ipynb            # Using tools / function calling with Claude
├── report.md                    # Sample long-form document used as test data
└── README.md
```

## Notebooks

| Notebook | Description |
|---|---|
| `Claude_With_API.ipynb` | Introduction to authenticating and making calls to the Claude API. |
| `Prompt_Engineering.ipynb` | Techniques for writing clear, effective prompts to improve Claude's responses. |
| `Prompt_Evaluation.ipynb` | Methods for systematically evaluating prompt/response quality. |
| `RAG_N_Agentic_Search.ipynb` | Building retrieval-augmented generation pipelines and agentic search workflows. |
| `Tools_Usage.ipynb` | Demonstrates Claude's tool use / function-calling capabilities. |

## Supporting Folders

- **`images/`** — Sample image files referenced by one or more notebooks (e.g., for vision-related exercises).
- **`pdf/`** — Sample PDF files used for document-processing or extraction exercises.
- **`report.md`** — A long-form sample document used as test/demo content (e.g., for summarization or RAG exercises).

## Getting Started

### Prerequisites

- A Google account (to run notebooks in Google Colab)
- An [Anthropic API key](https://console.anthropic.com/)
- Basic familiarity with Python and Jupyter/Colab notebooks

### Running the Notebooks

1. Open the repository on GitHub.
2. Click any `.ipynb` file, then choose **Open in Colab** (or open it manually via [colab.research.google.com](https://colab.research.google.com) → GitHub → paste the repo URL).
3. Set your Anthropic API key as an environment variable or Colab secret, e.g.:
   ```python
   import os
   os.environ["ANTHROPIC_API_KEY"] = "your_api_key_here"
   ```
4. Run the cells in order — each notebook is designed to be followed top to bottom.

## Usage

Each notebook can be run independently. If a notebook references files from `images/` or `pdf/`, download them into your Colab environment first (the repo's GitHub API can be used to fetch folder contents programmatically), then point the notebook's file paths to your local copies.