# BA Forage — Data Science Job Simulation

Work for the British Airways Data Science job simulation on Forage, covering two tasks: a lounge-eligibility lookup model and a booking-completion prediction model.

## Setup

```
uv sync
uv run jupyter lab
```

To work in VS Code instead of the browser: open a notebook, click **Select Kernel** (top right), and choose the `.venv` Python environment that `uv sync` created — VS Code should detect it automatically.

## Project structure

```
data/            Forage-provided datasets (gitignored — see data/README.md) + the lounge lookup template
notebooks/       task1.ipynb and task2.ipynb — all analysis and modelling
outputs/         Generated charts (feature importance, distributions) used in the summary
resources/       Forage-provided starter materials (Getting Started notebook, reference links)
summary.pdf      Manager-facing summary of task 2 findings
```

## Task 1 — Lounge eligibility

Estimated the percentage of Tier 1/2/3-eligible passengers by route type (long/short-haul) and time of day, using `data/flight_data.xlsx`. Output is `data/lounge_lookup_template.xlsx`, generated programmatically from `notebooks/task1.ipynb` so it can be regenerated whenever the underlying flight data changes.

## Task 2 — Predicting booking completion

Explored `data/customer_booking.csv`, engineered features (including a `booking_origin` → region grouping, validated against a frequency-encoding baseline), and trained a RandomForestClassifier to predict `booking_complete`. Evaluated via stratified cross-validation and a held-out test set (ROC-AUC, precision, recall, F1 — not accuracy, given the ~15% positive class rate). Findings are written up in `summary.pdf`.
