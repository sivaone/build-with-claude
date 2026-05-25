# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Educational repository for learning Claude API and LLM development patterns. Notebooks are numbered to indicate learning progression: basic requests → system prompts → streaming → structured data → prompt evaluation → tool use → RAG.

## Environment Setup

Requires a `.env` file with:
```
ANTHROPIC_API_KEY=...
VOYAGE_API_KEY=...   # needed for RAG/002_embeddings.ipynb and beyond
```

Activate the virtual environment before running anything:
```powershell
.venv\Scripts\Activate.ps1
```

## Running Notebooks

```powershell
jupyter notebook
```

Open any `.ipynb` file and run cells top-to-bottom — each notebook is self-contained.

## Project Structure

- **Root notebooks (`001`–`009`)** — Core Claude API patterns: requests, system prompts, streaming, structured data, prompt evaluation
- **`tools/`** — Tool/function calling with Claude (`010`–`013`)
- **`RAG/`** — Retrieval-Augmented Generation: chunking, embeddings (VoyageAI), vector DB, BM25, hybrid search

## Key Patterns

### Conversation management
Notebooks use a shared helper pattern:
```python
messages = []
add_user_message(messages, "...")
response = chat(messages)
add_assistant_message(messages, response)
```

### PromptEvaluator (`009_prompting_completed.ipynb`)
Full evaluation pipeline: generates diverse test cases, runs prompts concurrently via `ThreadPoolExecutor`, grades outputs with Claude, and produces an HTML report. The class methods are `generate_unique_ideas`, `generate_test_case`, `grade_output`, `run_evaluation`, and `generate_prompt_evaluation_report`.

### Tool use pattern
1. Define a Python function
2. Write its JSON Schema descriptor
3. Parse `tool_use` blocks from API responses
4. Append `tool_result` blocks back to the conversation

### RAG pipeline (`RAG/`)
Three chunking strategies (character, sentence, section) → VoyageAI embeddings → vector similarity or BM25 keyword search → hybrid retrieval. `RAG/report.md` is the sample document used across all RAG notebooks.
