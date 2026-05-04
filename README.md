# 🫀 Decision Analytic Model: Cost-Effectiveness of Angina Diagnosis & Treatment Strategies

[![Model Type](https://img.shields.io/badge/Model-Decision%20Tree-blue)]()
[![Language](https://img.shields.io/badge/Built%20in-Microsoft%20Excel-217346?logo=microsoft-excel)]()
[![Domain](https://img.shields.io/badge/Domain-Health%20Economics-red)]()
[![Perspective](https://img.shields.io/badge/Perspective-Cardiology%20Department-orange)]()
[![Sensitivity](https://img.shields.io/badge/Sensitivity-1--Way%20%26%202--Way-purple)]()

A fully worked decision analytic model evaluating the **cost-effectiveness of alternative diagnostic and treatment strategies** for patients referred with severe typical angina chest pain. Built in Microsoft Excel, the model includes a complete decision tree, ICER analysis, and both 1-way and 2-way deterministic sensitivity analyses.

---

## 📋 Overview

This model compares four management strategies for 55-year-old male patients referred to a Sheffield cardiology department with **severe typical angina**, using a decision tree framework with probabilistic inputs, discounted QALYs, and Net Monetary Benefit (NMB) decision rules.

### Strategies Evaluated

| Strategy | Description |
|---|---|
| **No Treatment (NT)** | Baseline — no diagnostic test or active treatment |
| **Medical Management (MM)** | Direct medical management without prior diagnostic testing |
| **Exercise Stress Test → optimal treatment (EST)** | Non-invasive imperfect test; followed by ANG or MM depending on result |
| **Coronary Angiography → optimal treatment (ANG)** | Invasive perfect test; followed by CABG or MM depending on disease confirmed |

### Treatment Options (post-diagnosis)
- No Treatment (NT)
- Medical Management (MM)
- Coronary Artery Bypass Graft Surgery (CABG)

---

## 🏥 Clinical Context

| Parameter | Value |
|---|---|
| Patient profile | Male, age 55, typical severe angina |
| Prior probability of disease P(S) | 0.90 |
| Disease states | No Disease (ND), Single Vessel Disease (SVD), Multi-Vessel Disease (MVD) |
| All patients | Good LV function, no prior MI |
| Model perspective | Sheffield cardiology department (direct costs only) |
| Time horizon | Remaining life expectancy (10–15 years) |
| Discount rate | 3.5% p.a. — applied to both costs and QALYs |
| Base-case CE threshold | £30,000/QALY |

---

## 🧮 Model Structure

The model is a **decision tree** structured as follows:

```
Patient with Severe Angina
│
├── No Test
│   ├── No Treatment
│   └── Medical Management ← optimal no-test strategy
│
├── Exercise Stress Test (EST)  [cost: £53]
│   ├── Test +ve (p = 0.775)
│   │   ├── Post-test P(disease) = 0.987
│   │   └── → Angiography (optimal) or MM
│   └── Test -ve (p = 0.225)
│       ├── Post-test P(disease) = 0.600
│       └── → MM (optimal given residual risk)
│
└── Coronary Angiography (ANG)  [cost: £668] ← overall optimal
    ├── No Disease (p = 0.10) → No Treatment (optimal)
    ├── SVD (p = 0.09) → CABG
    └── MVD (p = 0.81) → CABG
```

**Key structural assumptions:**
- Patients are treated in year 1; all costs are accumulated from then
- Doctors are assumed to be rational: positive test results always lead to appropriate follow-up
- Patients adhere to treatment for their remaining life expectancy
- Survival/death probabilities are applied post-diagnosis to avoid double counting

### CABG Branch Detail

```
CABG  [cost: £3,527]
├── Operative death (SVD: p=0.03 | MVD: p=0.08) → 0 QALYs
└── Alive → Surgical success (SVD: p=0.85 | MVD: p=0.70)
         ├── Success → Post-CABG MM  [£107/year]
         └── Failure → Standard MM  [£214/year]
```

---

## 📊 Key Inputs

### Probabilities

| Parameter | Value |
|---|---|
| Prior P(disease) — severe angina | 0.90 |
| P(SVD \| disease) | 0.10 |
| P(MVD \| disease) | 0.90 |
| EST Sensitivity P(+\|S) | 0.85 |
| EST Specificity P(-\|H) | 0.90 |
| P(T+) — total positives | 0.775 |
| P(T-) — total negatives | 0.225 |
| P(S\|T+) — TPPV | 0.987 |
| P(S\|T-) — FNPV | 0.600 |
| P(death \| ANG, ND) | 0.0002 |
| P(death \| ANG, SVD) | 0.0003 |
| P(death \| ANG, MVD) | 0.001 |
| P(death \| CABG, SVD) | 0.03 |
| P(death \| CABG, MVD) | 0.08 |
| P(surgical success \| CABG, SVD) | 0.85 |
| P(surgical success \| CABG, MVD) | 0.70 |

### Costs (GBP)

| Item | Cost | Type |
|---|---|---|
| Exercise Stress Test | £53 | One-time |
| Coronary Angiography | £668 | One-time |
| CABG Surgery | £3,527 | One-time (SVD or MVD) |
| Medical Management (severe angina) | £214/year | Annual |
| Post-CABG MM (successful surgery) | £107/year | Annual |
| Post-CABG MM (failed surgery) | £214/year | Annual |

**Discounted MM costs (3.5% p.a., 10-year horizon):**

| Scenario | Discounted Total |
|---|---|
| SVD, MM | £1,779.75 |
| MVD, MM | £966.22 |
| Healthy / No Treatment | £2,464.73 |
| Post-CABG success (SVD or MVD) | £889.88 |
| Post-CABG failure | £1,779.75 |

### Health Outcomes (QALYs — Rosser Matrix, discounted at 3.5%)

| Disease State | Treatment | Total Utility | Total QALYs |
|---|---|---|---|
| SVD | CABG | 9.799 | 1.416 |
| MVD | CABG | 6.241 | 1.426 |
| SVD | Medical Management | 9.066 | 1.382 |
| MVD | Medical Management | 4.424 | 1.238 |
| SVD | No Treatment | 4.900 | 0.976 |
| MVD | No Treatment | 1.400 | 0.654 |
| Healthy | MM / NT | 14.831 | 1.426 |

---

## 📈 Results

### Strategy-Level Expected Values

| Strategy | Expected QALYs | Expected Cost (£) | ENMB @ £30,000 |
|---|---|---|---|
| No Treatment | 2.602 | £0 | £78,074 |
| Medical Management | 5.075 | £1,189 | £151,071 |
| Exercise Stress Test | 5.573 | £4,308 | £162,885 |
| **Angiography** | **5.661** | **£4,855** | **£164,967** |

### ICER Table

| Comparison | ICER (£/QALY) |
|---|---|
| MM vs No Treatment | £481 |
| EST vs MM | £6,265 |
| ANG vs EST | £6,240 |

> All strategies lie on the efficiency frontier — no extended dominance applies. ICERs are low relative to any realistic NHS threshold.

### NMB Across Thresholds

| Strategy | £5,000 | £10,000 | £15,000 | £20,000 | £25,000 | £30,000 | £35,000 |
|---|---|---|---|---|---|---|---|
| No Treatment | £11,823 | £26,025 | £39,037 | £52,049 | £65,061 | £78,074 | £91,086 |
| Medical Management | £21,069 | £49,564 | £74,941 | £100,318 | £125,694 | £151,071 | £176,448 |
| EST | £23,011 | £51,423 | £79,288 | £107,154 | £135,019 | £162,885 | £190,750 |
| **Angiography** | **£28,304** | **£51,753** | **£80,056** | **£108,360** | **£136,663** | **£164,967** | **£193,270** |

> **Angiography has the highest NMB at every threshold tested**, making it the dominant strategy from £5,000/QALY upwards.

### Optimal Strategies by Testing Arm

**Direct Angiography arm:**

| Strategy | Exp. Cost | Exp. QALYs | ENMB |
|---|---|---|---|
| **ANG → CABG** | £4,541 | 4.521 | **145,648** |
| ANG → MM | £1,543 | 3.932 | 129,345 |
| ANG → NT (neg) | £668 | 11.399 | 34,130 |
| ANG → MM (neg) | £668 | 11.399 | 33,883 |

**EST arm:**

| Strategy | Exp. Cost | Exp. QALYs | ENMB |
|---|---|---|---|
| **EST → ANG → CABG** | £4,981 | 4.958 | **143,769** |
| EST → ANG → MM | £1,033 | 4.312 | 127,676 |
| EST → MM (pos only) | £826 | 3.460 | 102,943 |

**No-test arm:**

| Strategy | Exp. Cost | Exp. QALYs | ENMB |
|---|---|---|---|
| **No Test → MM** | £1,189 | 5.075 | **151,071** |
| No Test → NT | £0 | 2.602 | 78,074 |

**Overall top-four ranking:**

| Rank | Strategy | Exp. Cost | Exp. QALYs | ENMB |
|---|---|---|---|---|
| **1** | **ANG → CABG** | £4,541 | 4.521 | **145,648** |
| 2 | EST → ANG → CABG | £4,981 | 4.958 | 143,769 |
| 3 | ANG → MM | £1,543 | 3.932 | 129,345 |
| 4 | EST → ANG → MM | £1,033 | 4.312 | 127,676 |

**Optimal diagnostic strategy:** ANG (ENMB: 165,213) > EST (ENMB: 162,909)  
**Testing vs no testing:** Testing optimal (ENMB 165,213 vs 151,071)

---

## 🔍 Sensitivity Analysis

### 1-Way Sensitivity Analysis

One parameter was varied at a time across **7 cost-effectiveness thresholds** (£5k–£35k), recalculating NMB for all four strategies at each step.

#### Parameter 1 — Prior Disease Probability P(S): 0 → 1 (51 steps)

At **£10,000/QALY**, ANG overtakes MM as the optimal strategy at **P(S) ≈ 0.22**.

| P(S) range | Optimal strategy (λ = £10k) |
|---|---|
| 0.00–0.21 | Medical Management |
| **≥ 0.22** | **Angiography** |

Since our base case P(S) = 0.90 is far above the break-even, the recommendation is **highly robust to disease prevalence uncertainty**. The 1-way chart shows ANG and EST converging with MM only at very low prevalence, where the QALY gains from invasive diagnosis shrink.

#### Parameter 2 — QALY Discount Rate: 0% → 21% (22 steps)

ANG remains cost-effective at the £10,000 threshold up to a QALY discount rate of approximately **12–13%**, well above any standard rate in practice (3.5%).

| Discount rate | Optimal at λ = £10k |
|---|---|
| 0–~12% | ANG |
| > ~13% | MM |

**Cost discount rate:** Results are almost entirely insensitive — INMB values change by less than £10 across the full range tested.

---

### 2-Way Sensitivity Analysis

Four parameter pairs were varied simultaneously, producing strategy grids showing which strategy is optimal at each combination.

#### Grid 1 — QALY Discount Rate × CE Threshold (INMB grid)

ANG remains preferred as long as:
- CE threshold is at or above **£6,232/QALY** (break-even vs MM), and
- QALY discount rate is below **~12–13%**

Both conditions are comfortably satisfied under standard NHS assumptions.

#### Grid 2 — Cost Discount Rate × CE Threshold (INMB grid)

Strategy choice is virtually invariant to the cost discount rate across all thresholds, confirming cost discounting assumptions have no material effect on the recommendation.

#### Grid 3 — Disease Prevalence P(S) × CE Threshold (strategy grid)

| P(S) | £2,500 | £5,000 | £10,000 | £20,000 | £30,000 |
|---|---|---|---|---|---|
| 0.00 | NT | NT | NT | NT | NT |
| 0.05–0.20 | MM | MM | MM | EST | EST |
| 0.25–0.50 | MM | MM | EST | ANG | ANG |
| 0.60–0.80 | MM | MM | ANG | ANG | ANG |
| **0.90*** | **MM** | **MM** | **ANG** | **ANG** | **ANG** |
| 1.00 | MM | MM | ANG | ANG | ANG |

*Base case. At thresholds ≥ £10,000 and P(S) above ~0.22–0.60, ANG is recommended.

#### Grid 4 — EST Sensitivity × EST Specificity (strategy grid)

EST becomes dominant over ANG only when it approaches perfect accuracy (sensitivity and specificity both near 1.0). Even then, EST only dominates up to P(S) ≈ 0.94 — above this, the high disease burden means ANG's precision advantage outweighs the reduced procedural risk of EST. At the base case accuracy (sensitivity 0.85, specificity 0.90), ANG is preferred across virtually all prevalence levels.

#### Grid 5 — CABG Success Rate × CABG Cost (strategy grid)

| CABG success rate | Cost ~£1,000 | Cost ~£2,000 | Cost ~£3,000+ |
|---|---|---|---|
| < 0.85 | MM | MM | MM |
| 0.85–0.90 | ANG | ANG | MM |
| ≥ 0.90 | ANG | ANG | ANG |

At higher surgical costs, ANG+CABG requires a higher success rate to remain optimal.

### Key Break-Even Values Summary

| Parameter | Break-even value | Interpretation |
|---|---|---|
| CE threshold | **£6,232/QALY** | Below this, MM is preferred over ANG |
| Disease prevalence P(S) | **~0.22** | Above this, ANG preferred at £10k threshold |
| QALY discount rate | **~12–13%** | Above this, ANG advantage weakens |
| EST accuracy | Near-perfect (both ≥ 0.97) | Only then does EST compete with ANG |

**Overall conclusion:** Angiography followed by CABG where indicated is the recommended strategy for severe angina patients across virtually all plausible parameter combinations, provided the CE threshold is at or above **£6,232/QALY**.

---

## 📁 Repository Structure

```
angina-decision-model/
│
├── README.md                          # This file
├── Analytical_Decision_model.xlsx     # Full Excel model (8 sheets)
│
├── docs/
│   ├── case_study_brief.pdf           # Original case study specification
│   ├── model_methodology.md           # Bayesian revision, QALY calc, costs
│   └── rosser_matrix.md               # Utility values and health state profiles
│
├── results/
│   └── icer_summary.md                # ICER table and NMB results
│
└── sensitivity-analysis/
    └── sensitivity_notes.md           # 1-way and 2-way sensitivity findings
```

### Excel Workbook Sheet Guide

| Sheet | Contents |
|---|---|
| Title | Cover page |
| Introduction | Model background and context |
| Inputs | All probabilities — prior, conditional, joint, predictive (Bayes) |
| Costs & QALY | Rosser matrix, discounted QALY profiles, discounted cost tables |
| The Decision Tree | Full decision tree with expected costs, QALYs, and NMB by branch |
| ICER Interpretation | Strategy ranking, ICER table, NMB at 7 thresholds (£5k–£35k) |
| 1-Way Sensitivity | NMB tables for P(S) and QALY discount rate across all thresholds |
| 2-Way Sensitivity | Strategy grids for 5 parameter pairs including prevalence × threshold, EST accuracy, and CABG success |

---

## 🛠️ Methods Summary

- **Framework**: Decision tree (one-time irreversible treatment decision; no Markov transitions needed)
- **Probability revision**: Bayes' theorem for EST posteriors — TPPV, FNPV, and disease-type predictive values
- **QALY estimation**: Rosser Matrix (Williams, 1985) — year-by-year health state profiles, discounted at 3.5% p.a.
- **Cost discounting**: 3.5% p.a. over 10-year horizon (HM Treasury Green Book)
- **Decision rule**: Net Monetary Benefit — NMB = (λ × E[QALYs]) − E[Costs]
- **ICER**: Incremental analysis on efficiency frontier with dominance checks
- **1-way DSA**: P(S) varied 0→1 (51 steps); QALY discount rate 0→21% (22 steps); across 7 thresholds
- **2-way DSA**: QALY rate × threshold, cost rate × threshold, prevalence × threshold, EST sensitivity × specificity, CABG success rate × CABG cost
- **Software**: Microsoft Excel — fully formula-based, no macros, transparent and auditable

---

## 📚 References

- Williams A. (1985). Economics of coronary artery bypass grafting. *BMJ*, 291, 326–329.
- Drummond MF et al. (2015). *Methods for the economic evaluation of health care programmes* (4th ed.). Oxford Medical Publications.
- Briggs A., Claxton K. & Sculpher MJ. (2006). *Decision analytic modelling for the evaluation of health technologies*. Oxford University Press.
- Phelps CE & Mushlin AI. (1988). Focusing technology assessment using medical decision theory. *Medical Decision Making*, 8, 279–289.

---

## 👤 Author

Built as part of the **Evaluation of Health Care** module, demonstrating applied health economic modelling skills: decision tree construction, Bayesian probability revision, QALY estimation via the Rosser Matrix, cost-effectiveness analysis (ICER and NMB), and deterministic sensitivity analysis (1-way and 2-way).
