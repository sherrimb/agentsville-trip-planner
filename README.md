# AgentsVille Trip Planner 🧭🤖
**Agentic AI travel assistant** that generates and *refines* day-by-day itineraries for a (fictional) city, **AgentsVille**.  
It combines **structured prompting**, **Pydantic models**, a **ReAct revision agent** with tool use, and **LLM-powered evaluations** (including weather/activity compatibility).

<p align="center">
  <img src="assets/agent.jpg" alt="AgentsVille Trip Planner Banner" width="1000"/>
</p>

<p align="center">
  <img src="assets/img_7974.jpg" alt="Evaluation Screenshot 1" width="600"/>
  <br/>
  <img src="assets/img_7975.jpg " alt="Evaluation Screenshot 2" width="600"/>
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

Core Components
	•	Pydantic models: VacationInfo, Traveler, Activity, ItineraryDay, TravelPlan
	•	ItineraryAgent: Generates initial plan → JSON conforming to TravelPlan.model_json_schema()
	•	LLM Evals:
	•	start/end dates match
	•	total cost accurate & within budget
	•	interests coverage
	•	weather compatibility (uses ACTIVITY_AND_WEATHER_ARE_COMPATIBLE_SYSTEM_PROMPT)
	•	ReAct Revision Agent:
	•	Runs run_evals_tool → fixes issues (e.g., outdoor activity during storms) → runs evals again → calls final_answer_tool

⸻

🧪 Example Prompts (snippets)

Weather & Activity Compatibility (excerpt)
	•	Guideline: If an event explicitly moves indoors (e.g., “moves indoors to Harmony Hall if it rains”), treat as IS_COMPATIBLE even in thunderstorms.
	•	Final Answer: [IS_COMPATIBLE] or [IS_INCOMPATIBLE] (strict)

Itinerary Agent (excerpt)
	•	Role: Itinerary Planning Agent
	•	Task: Analyze weather + activities + interests; produce day-by-day plan; keep running cost; respect budget.
	•	Output: Two sections — ANALYSIS and FINAL OUTPUT where FINAL OUTPUT is valid TravelPlan JSON.

⸻

🚀 Getting Started

Requires Python 3.10+ and Jupyter.

	1.	Clone or download this repo, then open project_starter.ipynb in Jupyter.
	2.	API key: The notebook uses mocked tools; LLM calls require an OpenAI-compatible client.
	•	Set env var: export OPENAI_API_KEY=... (or use the cell provided in the notebook).
	3.	Run the notebook top-to-bottom:
	•	Fill VacationInfo
	•	Generate initial plan (ItineraryAgent)
	•	Run Evals
	•	Iterate with the ReAct Revision Agent until all evals pass ✅

Note: This repo includes mocked APIs for activities & weather to keep runs deterministic and reviewable.

⸻

📊 Reproducing the passing result
	•	Open project_starter.ipynb
	•	Run cells until evaluations; if any fail, the ReAct agent will:
	•	call get_activities_by_date_tool to fetch catalog
	•	update activities for weather/interest/budget
	•	re-run run_evals_tool
	•	Final cell prints:
All evaluation functions passed successfully for the revised travel plan.

⸻

🔧 Tools (docstrings summarized)
	•	calculator_tool: math/cost aggregation
	•	get_activities_by_date_tool(date: str, city: str): returns valid Activity dicts; date="YYYY-MM-DD"; city is case-sensitive
	•	run_evals_tool: runs the full evaluation suite
	•	final_answer_tool: ends the ReAct loop; returns final TravelPlan

⸻

🧱 Pydantic Models (examples)
	•	VacationInfo: destination, date_of_arrival, date_of_departure, budget, travelers: List[Traveler]
	•	TravelPlan: city, start_date, end_date, total_cost, itinerary_days: List[ItineraryDay]

⸻

📸 Screens & Demo

Add your screenshots to /assets and reference them here:
	•	Passing evals: assets/screenshot-itinerary-pass.png
	•	ReAct traces or prompt boxes: assets/screenshot-evals.png

⸻

🙋‍♀️ FAQ

Q: Why mock tools instead of hitting live APIs?
A: Keeps the project deterministic, fast to run, and reviewer-friendly while still demonstrating tool use.

Q: Where are the prompts?
A: In the notebook: ITINERARY_AGENT_SYSTEM_PROMPT, ACTIVITY_AND_WEATHER_ARE_COMPATIBLE_SYSTEM_PROMPT, and the ItineraryRevisionAgent system prompt.

⸻

📝 License

MIT

💌 Author

Sherri (SK) — Agentic AI enthusiast | AWS learner | Tech fan 🐬🌊
