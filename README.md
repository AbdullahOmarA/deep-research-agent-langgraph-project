# AI Research Agent

A multi-agent research system built with LangChain and LangGraph. The project takes a research question, gathers information from web sources, reviews the collected research, and produces a final report based on the available evidence.

The complete implementation is contained in `research_agent.ipynb`.

## Project Architecture

The system is built around three specialized agents:

- **Researcher:** responsible for web search and reading relevant webpages. It collects useful findings, keeps track of supporting sources, and reports disagreements when sources provide different information.
- **Analyst:** reviews the research notes and evaluates the information collected by the researcher. It looks for key findings, agreements, disagreements, missing information, and unsupported claims.
- **Writer:** uses the research and analysis to produce the final report in a clear format suitable for someone who is not familiar with the topic.

The agents are connected through a LangGraph `StateGraph`. The workflow can return to the research stage when the analyst identifies an important information gap.

```text
Research Question
       |
       v
  Researcher
       |
       v
    Analyst
     /   \
    /     \
More      Enough
Research  Information
  |           |
  v           v
Researcher   Writer
  |           |
  +-----> Analyst
              |
              v
         Final Report


```
The research loop has a maximum retry limit, so the system can request additional information without continuing indefinitely.


   ## Research Process

The researcher follows a simple source-based workflow:

1. Search for relevant sources.
2. Select the most useful results.
3. Read the actual webpages when possible.
4. Extract the important findings and supporting source information.
5. Stop once enough relevant information has been collected.

The researcher is also instructed to avoid repeatedly searching for the same information or reading the same webpage multiple times.

For important numerical claims, dates, rankings, targets, investments, and major announcements, the research prompt gives preference to primary or official sources when available.

## Reliability

Several controls were added to make the pipeline more reliable:

- `LoopDetector` monitors repeated researcher tool calls.
- Stage outputs are checked for stagnation or highly similar responses.
- A maximum step budget is applied to each agent run.
- Research retries are limited to two additional attempts.
- `InMemorySaver` is used as the graph checkpointer.
- When a loop or stagnant output is detected, a warning is printed instead of silently ignoring it.
- If the analyst identifies missing information, the graph can route the task back to the researcher rather than producing the final report immediately.

These checks are implemented as part of the pipeline rather than being separate demonstration code.

## Model and Tools

The project uses:

- **LangChain 1.x** for the agents and tool integration.
- **LangGraph** for the multi-agent workflow and state management.
- **OpenRouter** for model access.
- **DeepSeek V4 Flash** as the language model.
- `search_web` for finding relevant sources.
- `read_webpage` for reading webpage content.

The provided tracing system also records agent runs, token usage, and OpenRouter cost.

## Testing

The pipeline was tested with different research questions rather than relying on a single example.

The tests included comparison questions as well as current research questions. This was used to verify that the researcher was actually searching for relevant sources, reading webpages, passing the collected information to the analyst, and producing a final report through the complete pipeline.

The pipeline was also tested with different prompts to make sure the workflow was not dependent on one specific question and could complete the research process successfully.

## Running the Project


To test another question, change the query passed to `run_pipeline()`:

```python
pipeline_result = await run_pipeline(
    "Compare RAG and fine-tuning"
)
```
The returned result contains the final report and metadata about the pipeline run.

### Local Setup

```bash
uv sync
cp .env.example .env
uv run jupyter lab research_agent.ipynb

```
## Notebook Structure

| Section | Purpose |
| --- | --- |
| Setup | Model configuration, API key, and dependencies |
| Observability | Tracing utilities and loop detection |
| Research Tools | Web search, webpage reading, and URL validation |
| Shared Runner | Common agent execution and trace handling |
| Prompts | Instructions for each research stage |
| Agents | Researcher, analyst, and writer |
| Pipeline | LangGraph state, routing, retries, and checkpointing |
| Checks | Running the pipeline and inspecting the returned results |
| Challenges | Examples for loop detection and agent memory |
```
```
## Structure

```text
project Files
├── research_agent.ipynb   # # Main project notebook
├── EVALUATION.md          # Grading rubric for the project
├── pyproject.toml         # Dependencies (for local runs)
├── .env.example           # Environment variable template (local runs)
├── .gitignore             # Keeps .env and local caches out of git
└── uv.lock                # Locked dependency versions
```

Never commit .env because it contains the OpenRouter API key.

## Quick reference

```bash
uv sync                                  # install dependencies
uv run jupyter lab research_agent.ipynb  # open the project
```
Submitted by: Abdullah AlTurki — academy: [@SDAIAAcademy](https://github.com/SDAIAAcademy)

