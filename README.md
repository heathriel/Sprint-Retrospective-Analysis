# Sprint Retrospective Analysis

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/heathriel/Sprint-Retrospective-Analysis/HEAD)
[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/heathriel/Sprint-Retrospective-Analysis/blob/main/sprint_analysis.ipynb)
[![CI](https://github.com/heathriel/Sprint-Retrospective-Analysis/actions/workflows/test-notebook.yml/badge.svg)](https://github.com/heathriel/Sprint-Retrospective-Analysis/actions/workflows/test-notebook.yml)

> Workshop material from **[Agile AI: Leveraging AI to 100X Your Team's Productivity](https://www.kcdc.info/)** — KCDC 2024

Turn raw sprint data into a complete retrospective — EDA, ML-powered effort forecasting, and AI-generated coaching insights — in a single Jupyter notebook.

---

## What this does

| Step | Description |
|------|-------------|
| **1. Load** | Read sprint history from a CSV (works with Jira / Linear / Azure DevOps exports) |
| **2. Preprocess** | Compute time drift per task, encode team members |
| **3. Visualize** | Completion rates, estimated vs actual hours, per-sprint and per-member drift |
| **4. Predict** | Random Forest model forecasts effort for the next sprint before it starts |
| **5. AI Retro** | LLM synthesizes the metrics into a structured retrospective: what went well, what didn't, root causes, action items |

---

## Quickstart

### Option A — Zero install (Binder or Colab)

Click a badge above. No account or API key required — the LLM step gracefully skips if no key is set.

### Option B — Local

```bash
git clone https://github.com/heathriel/Sprint-Retrospective-Analysis.git
cd Sprint-Retrospective-Analysis
pip install -r requirements.txt
jupyter notebook sprint_analysis.ipynb
```

### Option C — With AI retrospectives enabled

Create a `.env` file in the repo root:

```bash
# Pick one — both use the same OpenAI-compatible interface
FIREWORKS_API_KEY=fw_...      # https://fireworks.ai  (fast, generous free tier)
OPENAI_API_KEY=sk-...         # https://platform.openai.com
```

The notebook auto-detects whichever key is present. Fireworks is recommended — it's faster and cheaper for this use case.

---

## Bring your own data

Replace `sprint_data.csv` with an export from your real project management tool.

**Required columns:**

| Column | Type | Description |
|--------|------|-------------|
| `sprint_id` | int | Sprint number |
| `team_member` | str | Name or ID of the person |
| `task_id` | int | Unique task identifier |
| `task_description` | str | Short description of the work |
| `estimated_hours` | float | Story point / hour estimate (blank = not yet estimated) |
| `actual_hours` | float | Real hours logged (blank = not yet complete) |
| `completion_status` | str | `Completed` or `In Progress` |

The notebook handles missing values gracefully — unestimated future tasks (like Sprint 6 in the sample) are kept for prediction but excluded from historical analysis.

---

## Repository structure

```
Sprint-Retrospective-Analysis/
├── sprint_analysis.ipynb   # Main notebook
├── sprint_data.csv         # Sample dataset (5 sprints, 3 team members)
├── requirements.txt        # Pinned Python dependencies
└── .github/
    └── workflows/
        └── test-notebook.yml   # CI: runs notebook on every push
```

---

## Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| pandas | ≥ 2.0 | Data loading and manipulation |
| numpy | ≥ 1.24 | Numeric operations |
| matplotlib | ≥ 3.7 | Plotting |
| seaborn | ≥ 0.12 | Statistical visualizations |
| scikit-learn | ≥ 1.3 | Random Forest, TF-IDF, preprocessing |
| openai | ≥ 1.0 | LLM API client (OpenAI-compatible) |
| python-dotenv | ≥ 1.0 | `.env` file support |

---

## Further explorations

- Trend analysis across 10+ sprints — is velocity drift trending up or down over time?
- Does task description length or vocabulary correlate with overrun probability?
- Multi-team comparison using the same pipeline on parallel squads
- Automatically post the AI retrospective to Confluence, Notion, or Slack
- Replace Random Forest with XGBoost or a fine-tuned LLM for effort estimation

---

## License

[GPL-3.0](LICENSE)
