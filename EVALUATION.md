# Student Project Evaluation Criteria: AI Agentic Systems

This rubric is used to evaluate student projects building AI Agent systems.
**You need at least 80 points to pass.**

Grading is weighted on **architecture**: the observability foundation
(tracing, token counting, real OpenRouter cost) is given in the notebook,
so what you build yourself is what earns the grade.

---

## Summary of Weighting

| Category | Points | Key Focus |
| :--- | :---: | :--- |
| **1. Agent Architecture** | 65 | Agent loop (create_agent), tool integration, multi-agent graph design |
| **2. Observability & Reliability** | 25 | What you ADD on top of the given tracing: loop detection, memory |
| **3. Engineering Excellence** | 10 | Dependency management, notebook structure, documentation |
| **Total** | **100** | |
| **Bonus** | +15 | Full RAG system integration |

---

## Detailed Rubric

### 1. Agent Architecture (65 Points + 15 Bonus)

*How well did you design the graph?*

> **A straight sequential chain (researcher → analyst → writer) cannot
> pass this project.** It is a fine first working version to build and
> test, but on its own it caps at the Satisfactory band — even with full
> marks everywhere else it stays below the 80-point pass line. You must
> change the graph: parallel branches, a retry/quality loop, a planner
> that splits the query, or conditional routing.

| Band | Points | Description |
| :--- | :---: | :--- |
| **Excellent** | 52–65 | Multi-agent graph with at least one non-sequential pattern: parallel stages (asyncio.gather or parallel StateGraph edges), a fact-check retry loop back to the researcher, a planner that decomposes the query, or conditional routing. Runs reliably and handles bad tool results gracefully. |
| **Good** | 39–51 | A non-sequential pattern is present and mostly works (retry loop, planner, parallel branches, or routing) but is buggy, incomplete, or unproven on a real query. |
| **Satisfactory** | 26–38 | Sequential pipeline (researcher → analyst → writer) that runs, or a single working agent. Cannot reach the pass line on its own. |
| **Poor** | 0–25 | Agent is a prompt wrapper. No real graph, no tool use, or it does not run. |

**Bonus (+15):** Implement a full RAG (Retrieval-Augmented Generation) system.

---

### 2. Observability & Reliability (25 Points)

*The tracing stub, token totals, and real OpenRouter cost are GIVEN — these
points are for what you add yourself.*

| Band | Points | Description |
| :--- | :---: | :--- |
| **Excellent** | 22–25 | LoopDetector wired into your pipeline (repetition AND stagnation) with a visible reaction (retry/warn), plus one reliability extension such as checkpointer memory or a step budget per stage. |
| **Good** | 16–21 | One LoopDetector strategy wired into the pipeline, or checkpointer memory added. |
| **Satisfactory** | 9–15 | Runs the given tracing and can explain the trace tree, token and cost numbers, but adds nothing. |
| **Poor** | 0–8 | Output/trace ignored or broken; cannot say what the agent spent. |

---

### 3. Engineering Excellence (10 Points)

*Is the work maintainable and professionally presented?*

| Band | Points | Description |
| :--- | :---: | :--- |
| **Excellent** | 9–10 | Uses `uv` for dependency management. Clear notebook structure with separated concerns (setup, tools, agents, pipeline, checks). Clean `README` with setup and usage instructions. |
| **Good** | 6–8 | Standard pip setup. Basic notebook organization. Decent documentation. |
| **Satisfactory** | 3–5 | Messy cells, duplicated code, vague setup instructions. |
| **Poor** | 0–2 | Chaotic structure. Hard-coded secrets. No README or setup instructions. |