# Farex — project context

## What this is
Farex is an evaluation scaffold for studying **routing failures in same-model, prompt-specialized
multi-agent setups**: one worker model, several specialist system prompts, and a "router" that picks
which prompt the worker receives. The research question: how does sending a task to the wrong specialist
prompt change task success, latency, and token usage? Findings must not be generalized to multi-agent
architectures with different models, tools, or multi-step delegation.

The project root is `Research/` (git repo, remote `origin` = `github.com/farex-ai/research`, branch `main`; moved from `koryei/farex` 2026-11-07).
The outer `Farex.dev/` folder maps to the GitHub organization `farex-ai`; see `../CLAUDE.md`.

## Current status (2026-10-31)
- **No valid routing experiment has been run yet.** All historical runs used matched routing (task → its own label).
- The pilot moved out of this repo into the sibling folder `../routing-pilot/` (plan, tasks, corrected prompts, code).
  This repo stays historical. The only approved change is the appended README note.

## Key decisions
- Pilot conditions: **reference** (author-assigned label, not proven-optimal) vs **fixed-wrong** (cyclic shift
  coding→math→extraction→evidence_qa→coding). LLM router and repetitions are deferred to Stage 3, after Stage 2 is reviewed.
- Score by task requirements only, never by which destination answered. Model-written code runs only in the sandbox described in `../routing-pilot/PLAN.md` §C.3.
- Token counts come from the provider or are marked unavailable. No `chars/4` estimates and no cost figures in the pilot.
- A null result is acceptable. Don't redesign prompts or tasks to force a routing effect.

## Rules for working in this repo
- **No paid API calls.** Local Ollama only, and only after the owner confirms which model is installed.
- **Don't read `.env`** (it holds a provider credential). The pilot code does not load it.
- **Historical artifacts are frozen.** Don't edit or delete `logs/`, `results/`, `README.md` claims,
  `routing_conditions.txt`, the clinical files, or the original `agents/*.txt`. Document problems in `../routing-pilot/PLAN.md` §1.3 instead.
- New work goes in new, versioned files. Ask before editing any existing file.
- Commit only when the owner asks.

## Known problems in historical material (details: `../routing-pilot/PLAN.md` §1)
- `agents/math_agent.txt`, `extraction_agent.txt`, and `evidence_qa_agent.txt` hold each other's prompts (since the first commit). Treated as unintended.
- Clinical prose scorer (`evaluate/evaluate.py:98-113`) passes wrong answers, so the "100% prose" results in `README.md` / `results/batch_summary.md` are not evidence of correctness.
- Raw JSONL logs are gitignored and absent from the remote.
- The clinical set (`tasks/clinical_tasks.json`, `evaluate/clinical_*.py`, `batch_eval.py`) is kept as historical exploratory material and isn't used for the pilot. It is not a clinical-safety benchmark.

## Map
- `evaluate/evaluate.py`: original generic runner and provider transport (Ollama / OpenRouter / Requesty)
- `evaluate/clinical_agents.py`, `clinical_benchmark.py`, `run_clinical.py`, `batch_eval.py`: clinical exploration (historical)
- `evaluate/plot_*.py`: historical figures
- `tasks/`, `agents/`, `logs/`, `results/`: data; historical files are read-only by convention
