# AI Research Agent — Project Starter

One self-contained notebook: **researcher → analyst → writer**. You build
the three agents with LangChain 1.x `create_agent` and chain them with
LangGraph's Graph API (`StateGraph`). Finish three TODOs — write the system
prompts (TODO #1), build the agents (TODO #2), build the pipeline (TODO #3).

## Google Colab (easiest)

1. Open `research_agent.ipynb` in Colab.
2. Add a secret named `OPENROUTER_API_KEY` (key icon in the left sidebar).
3. Finish the TODOs, then `Runtime → Run all`.

## On your own machine

```bash
uv sync
cp .env.example .env   # open .env and paste your OPENROUTER_API_KEY
uv run jupyter lab research_agent.ipynb
```

## How to submit

1. **Fork** this repository (Fork button, top-right on GitHub).
2. **Clone your fork**, open the notebook, and finish the three TODOs.
3. **Commit and push** your work to your fork:
   ```bash
   git add research_agent.ipynb README.md
   git commit -m "Finish research agent project"
   git push
   ```
   Never commit your `.env` file — it holds your API key (it is already in `.gitignore`).
4. **Tag the academy** so we can find your submission: edit the bottom of your fork's `README.md`, add this line, then commit and push again:
   ```markdown
   Submitted by: <your name> — academy: @SDAIAAcademy
   ```
5. Your submission is complete when your fork's last commit contains your finished `research_agent.ipynb` and the README line above. Grading follows `EVALUATION.md`.

## Structure

```
project_starter/
├── research_agent.ipynb   # the whole project (helpers given, 3 TODOs inside)
├── EVALUATION.md          # Grading rubric for the project
├── pyproject.toml         # Dependencies (for local runs)
├── .env.example           # Environment variable template (local runs)
├── .gitignore             # Keeps .env and local caches out of git
└── uv.lock                # Locked dependency versions
```

## Quick reference

```bash
uv sync                                  # install dependencies
uv run jupyter lab research_agent.ipynb  # open the project
```