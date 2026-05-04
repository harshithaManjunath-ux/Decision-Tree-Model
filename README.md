# 🫀 Decision Analytic Model: Cost-Effectiveness of Angina Diagnosis & Treatment Strategies

[![Model Type](https://img.shields.io/badge/Model-Decision%20Tree-blue)]()
[![Language](https://img.shields.io/badge/Built%20in-Microsoft%20Excel-217346?logo=microsoft-excel)]()
[![Domain](https://img.shields.io/badge/Domain-Health%20Economics-red)]()
[![Perspective](https://img.shields.io/badge/Perspective-Cardiology%20Department-orange)]()

A fully worked decision analytic model evaluating the **cost-effectiveness of alternative diagnostic and treatment strategies** for patients referred with severe typical angina chest pain. Built in Microsoft Excel as part of a health economic evaluation case study.

---

## 📋 Overview

This model compares four management strategies for 55-year-old male patients referred to a cardiology department with **severe typical angina**, using a decision tree framework with probabilistic inputs, discounted QALYs, and incremental cost-effectiveness analysis.

### Strategies Evaluated

| Strategy | Description |
|---|---|
| **No Treatment (NT)** | Baseline — no diagnostic test or active treatment |
| **Medical Management (MM)** | Direct medical management without prior diagnostic testing |
| **Exercise Stress Test → optimal treatment (EST)** | Non-invasive imperfect test, followed by optimal treatment given result |
| **Coronary Angiography → optimal treatment (ANG)** | Invasive perfect test, followed by optimal treatment given result |

### Treatment Options (post-diagnosis)
- No Treatment (NT)
- Medical Management (MM)
- Coronary Artery Bypass Graft Surgery (CABG)

---

## 🏥 Clinical Context

| Parameter | Value |
|---|---|
| Patient profile | Male, age 55, typical severe angina |
| Prior probability of disease | 0.90 |
| Disease states | No Disease (ND), Single Vessel Disease (SVD), Multi-Vessel Disease (MVD) |
| All patients | Good LV function, no prior MI |
| Model perspective | Sheffield cardiology department |
| Time horizon | Remaining life expectancy (10–15 years, discounted) |
| Discount rate | 3.5% (HM Treasury) — applied to both costs and QALYs |

---

## 🧮 Model Structure

The model is a **decision tree** with the following logic:

```
Patient with Severe Angina
│
├── No Test
│   ├── No Treatment
│   ├── Medical Management
│   └── (CABG requires prior ANG — not modelled as no-test option)
│
├── Exercise Stress Test (EST)  [cost: £53]
│   ├── Test +ve (p = 0.775)
│   │   ├── Post-test prob disease = 0.987
│   │   └── Optimal treatment: ANG → CABG (SVD/MVD) or No Treat (ND)
│   └── Test -ve (p = 0.225)
│       ├── Post-test prob disease = 0.600
│       └── Optimal treatment: MM (given residual disease probability)
│
└── Coronary Angiography (ANG)  [cost: £668]
    ├── No Disease (p = 0.10) → No Treatment / MM
    ├── SVD (p = 0.09) → CABG (preferred) or MM
    └── MVD (p = 0.81) → CABG (preferred) or MM
```

### CABG Branch (for SVD/MVD after confirmed diagnosis)
```
CABG  [cost: £3,527]
├── Operative death (SVD: p=0.03 | MVD: p=0.08) → 0 QALYs
├── Alive → Surgical success (SVD: p=0.85 | MVD: p=0.70)
│         ├── Success → Post-CABG MM  [£107/yr]
│         └── Failure → Standard MM  [£214/yr]
```

---

## 📊 Key Inputs

### Probabilities

| Parameter | Value |
|---|---|
| Prior P(disease) — severe angina | 0.90 |
| P(SVD \| disease, severe) | 0.10 |
| P(MVD \| disease, severe) | 0.90 |
| EST Sensitivity | 0.85 |
| EST Specificity | 0.90 |
| P(death \| ANG, SVD) | 0.0003 |
| P(death \| ANG, MVD) | 0.001 |
| P(death \| ANG, ND) | 0.0002 |
| P(death \| CABG, SVD) | 0.03 |
| P(death \| CABG, MVD) | 0.08 |
| P(surgical success \| CABG, SVD) | 0.85 |
| P(surgical success \| CABG, MVD) | 0.70 |

### Costs (GBP)

| Item | Cost |
|---|---|
| Exercise Stress Test | £53 |
| Coronary Angiography | £668 |
| CABG Surgery | £3,527 |
| Medical Management (severe) | £214/year |
| Post-CABG MM (successful) | £107/year |

### Health Outcomes (QALYs)

QALYs are derived from the **Rosser Matrix** (Williams, 1985), mapping health states across disability (I–VIII) and distress (A–D) dimensions, discounted at 3.5% per annum.

| Disease State | Treatment | Undiscounted Life Years | Total Discounted QALYs |
|---|---|---|---|
| SVD | CABG | 10 | 8.163 |
| MVD | CABG | 10 | 5.602 |
| SVD | MM | 10 | 7.605 |
| MVD | MM | 5 | 4.013 |
| SVD | No Treatment | 10 | 4.280 |
| MVD | No Treatment | 5 | 1.330 |
| No Disease | MM / NT | 15 | 11.401 |

---

## 📈 Results

### Expected Values by Strategy

| Strategy | Expected QALYs | Expected Cost (£) |
|---|---|---|
| No Treatment | 2.602 | £0 |
| Medical Management | 5.075 | £1,189 |
| Exercise Stress Test (→ ANG → CABG) | 5.573 | £4,308 |
| **Angiography (→ CABG)** | **5.661** | **£4,855** |

### Incremental Cost-Effectiveness

| Comparison | ICER (£/QALY) |
|---|---|
| MM vs No Treatment | £481 |
| EST vs MM | £6,265 |
| ANG vs EST | £6,240 |

### Net Monetary Benefit by Threshold

| Strategy | NMB @ £5,000 | NMB @ £10,000 | NMB @ £25,000 | NMB @ £35,000 |
|---|---|---|---|---|
| No Treatment | £11,823 | £26,025 | £65,061 | £91,086 |
| Medical Management | £21,069 | £49,564 | £125,694 | £176,448 |
| EST | £23,011 | £51,423 | £135,019 | £190,750 |
| **Angiography** | **£28,304** | **£51,753** | **£136,663** | **£193,270** |

> **Angiography dominates across all thresholds considered**, with the highest NMB in every scenario.

### Decision Rule

At all cost-effectiveness thresholds evaluated (£5,000–£35,000/QALY), the **optimal strategy is Angiography followed by CABG** where indicated (SVD or MVD), or No Treatment for patients with no disease.

The ANG strategy achieves the highest expected QALYs (5.661) and, despite the highest upfront cost, generates ICERs well below any reasonable threshold — reflecting the high disease burden and large QALY gains from correctly identifying and treating CAD.

---

## 📁 Repository Structure

```
angina-decision-model/
│
├── README.md                          # This file
├── Decision_Tree_Model.xlsx           # Full Excel model
│
├── docs/
│   ├── case_study_brief.pdf           # Original case study specification
│   ├── model_methodology.md           # Methods documentation
│   └── rosser_matrix.md               # Health state valuations used
│
├── results/
│   └── icer_summary.md                # ICER table and NMB results
│
└── sensitivity-analysis/
    └── sensitivity_notes.md           # Key sensitivity analysis findings
```

---

## 🔍 Sensitivity Analysis

Key parameters explored:

- **Discount rate**: Effect of applying 0% vs 3.5% to health benefits
- **CE threshold**: NMB calculated across £5,000–£35,000/QALY range
- **Prior disease probability**: Impact of lower disease prevalence (e.g. moderate pain cohort, p=0.60)
- **Surgical performance**: Effect of varying CABG mortality and success rates
- **Test accuracy**: Value of improving EST sensitivity/specificity

---

## 🛠️ Methods

- **Framework**: Decision tree (one-time decision, no Markov transitions)
- **Probability revision**: Bayes' theorem applied for EST posterior probabilities
- **QALY estimation**: Rosser Matrix health state valuations, year-by-year discounting at 3.5%
- **Economic rule**: Net Monetary Benefit (NMB) used to identify optimal strategy at each threshold
- **Dominance**: Extended dominance applied to the ICER table
- **Software**: Microsoft Excel (no macros — fully transparent formula-based model)


---

## 👤 Author

Built as part of the **Evaluation of Health Care** module — demonstrating applied health economic modelling skills including decision tree construction, Bayesian probability revision, QALY estimation via the Rosser Matrix, and cost-effectiveness analysis.
