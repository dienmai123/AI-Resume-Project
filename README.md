# AI Résumé Screening — Preliminary Results & Fairness Audits

Course: C S-4083/5083-001 (Fall 2025)
Group: Hiring 2
Authors: Dien Mai, Hoc Nguyen

This project tests if a résumé-screening model can rank candidates without disadvantaging protected groups. We use synthetic data, simple baselines, and clear fairness audits.

1) Goals
Build baseline models for résumé ranking.
Measure accuracy and fairness.
Run counterfactual tests (name swaps).
Produce 2–4 figures and one summary table for the milestone.

2) Guardrails
Use synthetic résumés. No real PII.
Do not train on first_name, city, or school.
Use them only for audits.
Log every experiment. Save seeds.

3) Repo Layout
data/
  raw/
  processed/
notebook/
  01_text_baseline.ipynb
  02_tabular_baseline.ipynb
reports/
  figs/
  model_card.md
  limits_next.md
  milestone_prelim.md
src/
  make_synth.py
  label.py
  split.py
  audit_groups.py
  fairness.py
  counterfactual.py
  utils.py
requirements.txt
README.md

4) Quick Start

# 1) Set up
python -m venv .venv
source .venv/bin/activate    # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# 2) Make synthetic data
python src/make_synth.py --n 1500 --out data/raw/resumes.csv --seed 4753

# 3) Label with a simple rule
python src/label.py --inp data/raw/resumes.csv --out data/processed/resumes_labeled.csv

# 4) Train/val/test split
python src/split.py --inp data/processed/resumes_labeled.csv --out data/processed --seed 4753

# 5) Run baseline notebooks (fills reports/figs with metrics)
- Open and run:
- notebook/01_text_baseline.ipynb
- notebook/02_tabular_baseline.ipynb

# 6) Create audit groups (proxies)
python src/audit_groups.py --inp data/processed/test.csv --out data/processed/test_with_groups.csv

# 7) Fairness metrics (DP gap, EO gap, calibration by group)
python src/fairness.py \
  --preds reports/text_baseline_preds.csv \
  --truth data/processed/test_with_groups.csv \
  --out reports/figs/fairness_text.json

# 8) Counterfactual name swap (text model)
python src/counterfactual.py \
  --model reports/text_baseline.model \
  --sample data/processed/test_with_groups.csv \
  --out reports/figs/counterfactual_text.csv

# 9) Compile milestone doc
# Fill reports/milestone_prelim.md with Table A + 2–4 figures.

5) Data schema (synthetic)
- Input fields (examples):
years_experience (int)
degree (categorical, e.g., HS/BA/MS/PhD)
gpa (float)
skills (list → joined text)
job_titles (list → joined text)
certifications (count)
employment_gaps (months)
city, school (audit only)
first_name, last_name (audit only)

- Labels:
score (0–100)
shortlist (binary; top 30% = 1)

6) Models
- Text baseline: TF-IDF → Logistic Regression (shortlist) and Linear Regression (score).
- Tabular baseline: Gradient Boosting on structured fields.
- Stretch (optional): small transformer encoder.

7) Fairness audits

- Compute on the test split.

- Selection rate (group g)
- SR_g = (# with ŷ=1 in g) / (# in g)

- Demographic Parity (DP) gap
- DP_gap = max_g SR_g − min_g SR_g

- True Positive Rate (TPR) per group
- TPR_g = (# with ŷ=1 and y=1 in g) / (# with y=1 in g)

- Equal Opportunity (EO) gap
- EO_gap = max_g TPR_g − min_g TPR_g

- Calibration by group
- Bin predicted scores; compare mean predicted vs. empirical success for each group.

- Counterfactual name swap
- Swap only first_name between name sets A/B; measure Δscore and Δshortlist.