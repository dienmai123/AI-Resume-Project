Submission Brief — AI Ethics Audit (Short)

Use this short document as the content to paste into your Google Doc. Insert the screenshots listed below at the indicated points (filename and caption provided).

1) Title & One-line summary

Title: AI Ethics Audit — Updated Preliminary Results
One-line summary: Re-ran baselines with two small model improvements (text: TF-IDF ngrams + C=0.5; tabular: GB n_estimators=150, max_depth=4). Text accuracy +3.5% and tabular F1 +91%; fairness audits reveal a gender TPR gap in tabular model.

2) Short Methods (2–3 lines)
- Text model: TF-IDF (n=1–3), LogisticRegression (C=0.5)
- Tabular model: GradientBoostingClassifier/Regressor (n_estimators=150, max_depth=4)
- Data: Synthetic résumé dataset; test set n=226 with demographic labels

3) Key quantitative results (very short table / bullets)
- Text (test): Accuracy 54.0% → 57.5% (+3.5%); F1 ~ 34%
- Tabular (test): Accuracy 70.8% → 65.9% (↓), F1 10.8% → 20.6% (+91%)
- Notes: Regression RMSEs increased slightly; classification prioritized.

4) Fairness headline (1 paragraph)
- Demographic parity gaps remain small (≤~6%). However, equalized odds (TPR) shows a concerning gender gap in the tabular model (EO gap ≈ 0.26): female TPR ≈ 6.9% vs male TPR ≈ 18.2%. This implies the tabular model misses many qualified female candidates and needs fairness mitigation before deployment.

5) Recommendations (short bullets)
- Immediate: Run threshold optimization or equalize TPR by group as post-processing.
- Near-term: Root-cause analysis of features driving gender gap; consider re-weighting or adversarial debiasing.
- Longer-term: Validate on real-world résumés and add automated fairness monitoring.

6) Where to place screenshots (Google Doc insertion guide)

Insert screenshots in this order — copy the file from `ai-resume-audit/reports/figs/` into your Doc and use the captions below.

A. After "Short Methods" — (one small figure, left or inline)
- File: `selection_rate_text_name_set.png`
- Caption: "Selection rate by name set (text model)."
- Purpose: show selection-rate parity across name groups for text model.

B. After "Key quantitative results" — (two figures side-by-side if possible)
- File 1: `selection_rate_tabular_name_set.png`
- Caption 1: "Selection rate by name set (tabular model)."
- File 2: `calibration_tabular_name_set.png`
- Caption 2: "Calibration curve (tabular) by name set — shows under-confidence."
- Purpose: evidence of selection rates and calibration differences that relate to model behavior.

C. After "Fairness headline" — (two figures, stacked)
- File 1: `selection_rate_text_gender_proxy.png`
- Caption 1: "Selection rate by gender (text model)."
- File 2: `selection_rate_tabular_gender_proxy.png`
- Caption 2: "Selection rate by gender (tabular model) — low overall selection and TPR differences."
- Purpose: show selection rates by gender across models.

D. Immediately after the previous pair — (two calibration plots)
- File 1: `calibration_text_gender_proxy.png`
- Caption 1: "Calibration by gender (text model)."
- File 2: `calibration_tabular_gender_proxy.png`
- Caption 2: "Calibration by gender (tabular model) — indicates probability scaling differences."
- Purpose: support claim about under/over-confidence and how it may affect selection.

E. Intersectional evidence (optional, add if space)
- File: `selection_rate_text_gender_name_set.png`
- Caption: "Selection rate by intersectional groups (text model)."
- File: `selection_rate_tabular_gender_name_set.png`
- Caption: "Selection rate by intersectional groups (tabular model)."
- Purpose: show intersectional disparities (if you want to highlight more nuance).

7) Recommended screenshot sizing and placement tips
- Use two-column layout for the side-by-side figures under "Key quantitative results".
- For stacked calibration plots, use full width but keep height ~200–300 px so the Doc stays compact.
- Add each image caption under the image in italics and small font.

8) Short closing paragraph (1–2 lines)
- "We improved model performance with modest hyperparameter changes and ran a full fairness audit; results are ready for submission. Key outstanding work: fairness mitigation for the tabular model's gender TPR gap."

---

File created: `/workspaces/AI-Resume-Project/SUBMISSION_BRIEF.md`

What I did next: I saved this concise brief in the repo root. Next I can (choose one):
- produce a copy formatted for Google Docs (e.g., a .docx) and include images inline, or
- open `NARRATIVE_SECTIONS.md` and extract a one-paragraph executive summary for the top of your Doc.

Tell me which you want me to do next (convert to .docx and embed images, or prepare a ready-to-paste Google Doc section with image placeholders and captions).