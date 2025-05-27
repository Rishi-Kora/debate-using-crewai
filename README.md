# Debate using crewai

A lightweight framework for running structured debates with AI agents using the CrewAI library.
In a nutshell, you define two “debater” agents (pro/con) and a “judge” agent, wire them together in a **Crew**, then kick off a debate on any motion.

---

## Table of Contents

* [Features](#features)
* [Demo](#demo)
* [Getting Started](#getting-started)

  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
  * [Configuration](#configuration)
* [Usage](#usage)
* [Project Structure](#project-structure)
* [How It Works](#how-it-works)

  * [Agents](#agents)
  * [Tasks](#tasks)
  * [Process](#process)
* [Outputs](#outputs)
* [Contributing](#contributing)
* [License](#license)

---

## Features

* **Modular**: Easily swap in new agents or tasks via YAML configs.
* **Extensible**: Define arbitrary debate motions at runtime.
* **Multi-model**: Supports different LLMs per agent (e.g., GPT-4O, Claude).
* **Sequential workflow**: Propose → Oppose → Decide.

---

## Demo

```bash
$ python main.py
```

```text
> Running Debate crew with motion: “There needs to be strict laws to regulate LLMs”
> [Propose] A compelling case FOR the motion…
> [Oppose] A compelling case AGAINST the motion…
> [Decide] Based on arguments, the winner is…
```

---

## Getting Started

### Prerequisites

* Python 3.10+
* An OpenAI API key (for GPT-4O-mini)
* An Anthropic API key (for Claude-3.7)
* `pip` or `poetry` for dependency management

### Installation

1. **Clone the repo**

   ```bash
   git clone https://github.com/Rishi-Kora/debate-using-crewai.git
   cd debate-using-crewai
   ```

2. **Install dependencies**
   Using `pip`:

   ```bash
   pip install -r requirements.txt
   ```

   Or with `poetry`:

   ```bash
   poetry install
   ```

3. **Set environment variables**

   ```bash
   export OPENAI_API_KEY="your-openai-key"
   export ANTHROPIC_API_KEY="your-anthropic-key"
   ```

---

## Configuration

All agent and task settings live in YAML under the project root:

* **agents.yaml**

  ```yaml
  debater:
    role: "A compelling debater"
    goal: "Present a clear argument either in favor of or against the motion: {motion}"
    backstory: "You're an experienced debater…"
    model: openai/gpt-4o-mini

  judge:
    role: "Decide the winner of the debate…"
    goal: "Given arguments for and against: {motion}, judge purely on merit"
    backstory: "You are a fair judge…"
    model: anthropic/claude-3-7-sonnet-latest
  ```

* **tasks.yaml**

  ```yaml
  propose:
    description: "Propose the motion: {motion}, build a convincing case FOR it"
    agent: debater
    output_file: output/propose.md

  oppose:
    description: "Oppose the motion: {motion}, build a convincing case AGAINST it"
    agent: debater
    output_file: output/oppose.md

  decide:
    description: "Review both arguments and decide which is more convincing"
    agent: judge
    output_file: output/decide.md
  ```

Edit `{motion}` at runtime via `main.py` or by passing your own inputs.

---

## Usage

1. **Modify** the motion in `main.py` (or adapt for CLI).
2. **Run**:

   ```bash
   python main.py
   ```
3. **Review** outputs in the `output/` directory:

   * `propose.md`
   * `oppose.md`
   * `decide.md`

---

## Project Structure

```
.
├── main.py               # Entry point for running a debate :contentReference[oaicite:0]{index=0}
├── crew.py               # Defines Debate crew, agents & tasks :contentReference[oaicite:1]{index=1}
├── agents.yaml           # AI agent configurations
├── tasks.yaml            # Task definitions & outputs
├── requirements.txt      # Python dependencies
└── output/               # Generated debate artifacts
```

---

## How It Works

### Agents

* **Debater**

  * Role: crafts arguments 👥
  * Model: GPT-4O-mini (OpenAI)
  * Config in `agents.yaml`

* **Judge**

  * Role: evaluates and declares a winner 🏆
  * Model: Claude 3-7 (Anthropic)
  * Config in `agents.yaml`

### Tasks

* **propose**: Debater constructs a pro‐motion argument
* **oppose**: Debater constructs a con‐motion argument
* **decide**: Judge compares and selects the winning side

All tasks are wired to agents via `tasks.yaml`.

### Process

We assemble a **Crew** (via `crewai.Crew`) that runs tasks **sequentially**:

1. `propose`
2. `oppose`
3. `decide`

Set `verbose=True` to see progress logs.

---

## Outputs

After running, you’ll find:

* `output/propose.md` – the “FOR” argument
* `output/oppose.md` – the “AGAINST” argument
* `output/decide.md` – judge’s verdict and reasoning

Each file contains a clear, concise markdown summary of its step.

---

## Contributing

1. Fork the repo
2. Create a branch (`git checkout -b feature/xyz`)
3. Make your changes & add tests
4. Submit a PR against `main`

Please respect code style (PEP 8) and write clear commit messages.

---

## License

This project is released under the **MIT License**.
Feel free to use, modify, and distribute!

---

Happy debating!
