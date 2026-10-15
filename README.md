# Agentic Systems Lab

**End goal:** show, with real projects, that you can build and verify complex agent systems and understand how they work underneath. Proof: this repo, with code, evals, and a write-up.

**Cost:** $0. Main API: Gemini (AI Studio). Backup: Groq. Optional fully local: Ollama. Free-tier limits change, so check current terms when you sign up.

## Layout

    agentic-systems-lab/
    ├── README.md
    ├── .env.example
    ├── 01_loop/              Phase 1: bare agent loop + bash tool
    ├── 02_evals/             Phase 2: tasks + pass-rate runner
    ├── 03_patterns/          Phase 3: chaining, routing, evaluator-optimizer
    ├── 04_supervisor/        Phase 4: supervisor + workers + test gate
    ├── 05_context_tools/     Phase 5: skill loader + MCP server
    ├── 06_capstone/          Phase 6: real task + write-up
    └── docs/
        └── agent-failure-log.md

## Resources

| Resource | Link |
|---|---|
| Anthropic: Building Effective Agents | https://www.anthropic.com/engineering/building-effective-agents |
| DeepLearning.AI: AI Agents in LangGraph (optional, after Phase 4) | https://www.deeplearning.ai/short-courses/ai-agents-in-langgraph/ |
| Aider repo | https://github.com/Aider-AI/aider |
| MCP specification | https://modelcontextprotocol.io/specification |
| Gemini API keys (free) | https://aistudio.google.com/apikey |
| Groq keys (free) | https://console.groq.com/keys |

Verify links are still current before relying on them.

## Progress

- [ ] Phase 1: The loop
- [ ] Phase 2: Evals
- [ ] Phase 3: Patterns
- [ ] Phase 4: Supervisor and workers
- [ ] Phase 5: Context and tools
- [ ] Phase 6: Capstone

---

## Phase 1: The loop (1 to 2 evenings) -> `01_loop/`

- [ ] Be the loop yourself in a chat: tell a model to reply with only terminal commands, run them, paste the output back. Do 4 or 5 rounds.
- [ ] Get a free API key, copy `.env.example` to `.env`, and make one call from Python that prints a reply.
- [ ] Add a `bash` tool definition and print the model's tool request.
- [ ] Wrap it in a `while` loop: run the command, feed the output back, stop when the model replies with plain text.
- [ ] Add a timeout, an iteration cap, and a refusal for dangerous commands.

**Done when:**
- [ ] "Create hello.txt containing hi" works from the terminal.
- [ ] The timeout is shown to trigger (e.g. a `sleep 100` command).
- [ ] The iteration cap is shown to trigger.
- [ ] No API key appears anywhere in the repo.

## Phase 2: Evals (1 to 2 evenings) -> `02_evals/`

- [ ] Write 3 to 5 tasks that a script can check (file contents, a `pytest` exit code).
- [ ] Write a runner that executes each task 10 times in a scratch folder and prints the pass rate.
- [ ] Include one task the agent should refuse.

**Done when:**
- [ ] You have a recorded baseline pass rate.
- [ ] You improved it at least once by changing the prompt or loop, and wrote down what changed.

## Phase 3: Patterns (about 1 week) -> `03_patterns/`

- [ ] Read "Building Effective Agents" now. Note where it describes mistakes you made.
- [ ] Build prompt chaining and routing as small experiments on top of your loop.
- [ ] Build evaluator-optimizer: one agent writes, another critiques, the loop stops when a test passes.

**Done when:**
- [ ] For each pattern you can say when it beats a single agent, backed by a measured result.

## Phase 4: Supervisor and workers (2 to 3 weeks, the main event) -> `04_supervisor/`

- [ ] A supervisor splits a task into a JSON plan of subtasks.
- [ ] Each subtask goes to a separate worker with its own prompt and scratch folder.
- [ ] A test run, never an LLM opinion, decides whether each result is accepted.
- [ ] On failure, feed the test output back to the worker, retry up to a limit, then escalate.
- [ ] Save run state to a JSON file.
- [ ] Add parallel workers once the sequential version works.

**Done when:**
- [ ] It finishes a multi-file task on a sample repo.
- [ ] A forced impossible subtask fails cleanly after the retry limit and is not merged.
- [ ] The merged result passes the full test suite when run independently.
- [ ] The gate code contains no LLM call.

## Phase 5: Context and tools (1 to 2 weeks) -> `05_context_tools/`

- [ ] Skills: keep short descriptions in the prompt, load full markdown instructions through a `load_skill` tool only when the task needs them.
- [ ] Log tokens per run; show loading 1 skill uses fewer tokens than loading all of them.
- [ ] Write one small MCP server and have your agent call it.

**Done when:**
- [ ] An unrelated task loads zero skills.
- [ ] Adding a new skill folder needs no code changes.
- [ ] The agent successfully uses your MCP tool.

## Phase 6: Capstone (1 to 2 weeks) -> `06_capstone/`

- [ ] Point the whole system at a real task: a course project or a bug in an open-source repo.
- [ ] Write up what broke, how you caught it, and what you changed, with before-and-after pass rates.
- [ ] Skim the Aider repo and compare its design to yours.

**Done when:**
- [ ] The repo is public with a README you could walk a recruiter through in five minutes.

---

## Alongside every phase

Use Claude Code, Cursor, or Gemini CLI on real coursework and log failures in `docs/agent-failure-log.md`.

## Rules

1. One phase at a time, in order.
2. Tick a box only after you've run the check.
3. Commit at the end of each phase: `git commit -m "phase-N: <summary>"`.
4. If time runs short, cut MCP first, never the evals.
