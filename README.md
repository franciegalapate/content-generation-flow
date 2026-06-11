# Guide Creator Flow

A CrewAI-powered flow that generates comprehensive, audience-tailored guides on any topic using local LLMs via Ollama.

## Overview

This project uses a CrewAI Flow to orchestrate a multi-step guide creation process:

1. **User Input** — prompts for a topic and audience level (beginner/intermediate/advanced)
2. **Outline Generation** — uses a direct LLM call to produce a structured JSON outline
3. **Content Writing** — a CrewAI content crew writes each section sequentially, with context from previous sections
4. **Compilation** — assembles all sections into a single Markdown guide

Output is saved to the `output/` directory:

- `output/guide_outline.json` — the structured outline
- `output/complete_guide.md` — the final compiled guide

## Requirements

- Python >=3.10, <3.13
- [uv](https://docs.astral.sh/uv/) for dependency management
- [Ollama](https://ollama.com) running locally with `llama3.2` pulled

## Installation

1. Install `uv` if you haven't already:

```bash
pip install uv
```

2. Install project dependencies:

```bash
crewai install
```

3. Pull the required Ollama model:

```bash
ollama pull llama3.2
```

4. Make sure Ollama is running:

```bash
ollama serve
```

> If you see `address already in use`, Ollama is already running — that's fine.

## Running the Project

From the root of the project:

```bash
uv run kickoff
```

You'll be prompted to enter a topic and audience level, then the flow will generate your guide automatically.

## Configuration

The LLM is configured to use Ollama locally in `src/guide_creator_flow/main.py`:

```python
llm = LLM(
    model="ollama/llama3.2",
    base_url="http://localhost:11434"
)
```

To use a different model, change `ollama/llama3.2` to any model you have pulled (check with `ollama list`).

To customize agents and tasks, edit:

- `src/guide_creator_flow/crews/content_crew/config/agents.yaml`
- `src/guide_creator_flow/crews/content_crew/config/tasks.yaml`

## Project Structure

```
guide_creator_flow/
├── src/guide_creator_flow/
│   ├── main.py                  # Flow definition and LLM logic
│   └── crews/content_crew/      # CrewAI crew for writing sections
│       ├── content_crew.py
│       └── config/
│           ├── agents.yaml
│           └── tasks.yaml
├── output/                      # Generated guides appear here
├── pyproject.toml
└── README.md
```
