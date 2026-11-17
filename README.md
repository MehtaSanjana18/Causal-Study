# Night Shift Work and Cardiometabolic Health in Indian Manufacturing Workers

**ES 678 Course Project — IIT Gandhinagar**
Sanjana Mehta | Roll No: 26210128

---

## What this project is about

India has tens of millions of workers on rotating or permanent night shifts across textile, pharmaceutical, and steel factories. Research from other countries shows that disrupting the body's circadian rhythm over time raises blood pressure and blood sugar. But for Indian workers, almost no causal evidence exists — most studies just show that night shift workers have worse health, without separating the effect of the shift itself from background differences like age, poverty, and where they live.

I wanted to know whether working in a manual occupation — which I use as a proxy for potential night shift exposure — actually *causes* higher blood pressure and blood sugar, not just whether the two are correlated. Manual workers tend to be younger, more urban, and from different wealth backgrounds than agricultural workers, so a simple comparison would give the wrong answer.

I used Double Machine Learning (DML), a causal inference method that removes confounding using Random Forest rather than assuming all relationships are linear. The key advantage over ordinary regression is that DML can handle the non-linear ways that age, wealth, and urban residence relate to blood pressure — and OLS cannot.

The analysis uses NFHS-5 data with clinician-measured blood pressure and fasting glucose on 30,810 working-age men. I compare manual occupation workers (occupation code 7) against agricultural workers (occupation code 6) across five industrial zones covering all 36 Indian states and union territories.

**Important:** NFHS-5 does not record actual shift schedules. Occupation code 7 is used as a proxy for night shift exposure based on the reasoning that Indian formal manufacturing runs 24/7 while agricultural work is predominantly daytime. Not every manual worker works nights. The estimated effect is an occupational group difference — night shift exposure is the proposed mechanism, not a confirmed fact. This makes the ATE a conservative lower bound.

---

## Repository

**GitHub:** [https://github.com/MehtaSanjana18/Causal-Study](https://github.com/MehtaSanjana18/Causal-Study)

```
├── DML_BP_analysis.ipynb    ← main analysis notebook
├── README.md                ← this file
└── results/
    ├── eda_plots.png
    ├── overlap_analysis.png
    ├── linearity_check.png
    ├── cate_analysis.png
    ├── shap_cate.png
    └── placebo_distribution.png
```

---

## Data access

I used the **NFHS-5 Men's Recode (2019--21)** — freely available at [dhsprogram.com](https://dhsprogram.com) after free academic registration.

1. Register at [dhsprogram.com](https://dhsprogram.com) — free for academic use
2. Request access to India 2019-21 Men's Recode (approved within a few hours)
3. Download `IAMR7EFL.DTA` (Stata format)
4. Place it in Google Drive at `My Drive/IAMR7EDT/IAMR7EFL.DTA`

No private data or restricted credentials are used anywhere in this project.

---

## Setup and rebuild steps

Everything runs in Google Colab — no local setup needed.

1. Get the data and place it at `My Drive/IAMR7EDT/IAMR7EFL.DTA`
2. Open `DML_BP_analysis.ipynb` in Google Colab
3. **Run Cell 1 first** — sets the random seed to 42
4. Runtime → Restart and Run All
5. Total runtime approximately 60--75 minutes

Install command (Cell 2 does this automatically):
```bash
pip install pyreadstat econml -q
```

**Environment:**
```
Python       3.10
econml       0.15
scikit-learn 1.3
shap         0.43
Random seed  42
```

---

## Code location

All analysis is in `DML_BP_analysis.ipynb`. Run Cell 1 first to fix the random seed, then run all cells in order.

The main result — DML ATE of +0.644 mmHg — is produced by Cell 13. To reproduce just the primary result, run Cells 1 through 13.

Split details: `KFold(n_splits=5, shuffle=True, random_state=42)`

---

## Main results

| Method | BP ATE | 95% CI | Sig |
|--------|--------|--------|-----|
| Raw difference | +0.349 mmHg | — | ~ |
| OLS adjusted | +0.776 mmHg | — | * |
| **DML W-only (primary)** | **+0.644 mmHg** | **[0.295, 0.994]** | **\*** |
| Doubly Robust | +0.793 mmHg | [0.465, 1.122] | * |
| IPTW trimmed | +0.820 mmHg | [0.433, 1.230] | * |

Glucose ATE (W-only): +1.032 mg/dL (95% CI: 0.146--1.918), significant.

---

## AI use disclosure

Claude was used for brainstorming the research design, understanding DML and causal inference concepts, and debugging code.

**External references:**
- Chernozhukov et al. (2018) — Double/debiased machine learning
- Wager and Athey (2018) — Causal forests
- Bang and Robins (2005) — Doubly robust estimation
- Kaur et al. (2024, ACM COMPASS) — Farm ponds causal ML
- NFHS-5 Men's Recode (IIPS and ICF, 2021)
