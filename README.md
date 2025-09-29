# AgentsVille Trip Planner 🧭🤖
**Agentic AI travel assistant** that generates and *refines* day-by-day itineraries for a (fictional) city, **AgentsVille**.  
It combines **structured prompting**, **Pydantic models**, a **ReAct revision agent** with tool use, and **LLM-powered evaluations** (including weather/activity compatibility).

<p align="center">
  <img src="assets/screenshot-itinerary-pass.png" alt="Passing evals" width="85%"/>
</p>

## ✨ Highlights
- **Expert Planner (ItineraryAgent):** Produces a structured `TravelPlan` JSON that validates against Pydantic models.
- **ReAct Revision Agent:** Uses THINK → ACT → OBSERVE cycles to fetch activities, run evals, and revise the plan.
- **Tooling:** calculator, get_activities_by_date, run_evals, final_answer (with explicit JSON call format).
- **LLM Evals:** Checks dates/city, cost accuracy, interests coverage, **and weather vs. activities** via a dedicated system prompt.
- **Prompt Engineering:** Role, Task, Output Format, Context, and targeted Examples to elicit reliable structured outputs.

---

## 🧩 What this project demonstrates 
- Designing **agentic systems** (multi-agent orchestration & feedback loops)
- **Reasoning-focused prompts** (CoT/ReAct) with deterministic output shapes
- **Structured validation** with **Pydantic** (enums, nested models, dates)
- **LLM tool use** (clear schemas + docstrings → fewer hallucinations, cleaner calls)
- **Eval-driven iteration** (fixing failures programmatically & via prompts)

---

## 🗺️ Project Architecture

```text
.
├── project_starter.ipynb      # Main Jupyter app: prompts, agents, evals, runs
├── project_lib.py             # Pydantic models, mocked tools, utilities
├── assets/                    # Screenshots for README
│   ├── screenshot-itinerary-pass.png
│   └── screenshot-evals.png
├── README.md
├── LICENSE
└── .gitignore
