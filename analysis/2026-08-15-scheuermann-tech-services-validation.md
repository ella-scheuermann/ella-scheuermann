# Stage 5 Validation Report

**Scenario:** U.S. Tech Services Firm – EUR 12,500,000 Receivable

**Author:** Ella Scheuermann

**Date:** August 15, 2026

---

# 1. Independent LLM Execution

## Prompt Used

A fresh Claude conversation was used for this validation. Only the committed Stage 2 FX Hedging Model Specification and the Stage 4 Market Data Memo were provided. No workbook, calculations, or additional guidance were supplied during the session. The model was instructed to independently calculate the hedge outcomes, compare the available strategies, and recommend the most appropriate hedge based solely on the provided documentation.

## Documents Provided

- Stage 2 FX Hedging Model Specification
- Stage 4 Market Data Memo

No additional documents, workbook outputs, or corrections were provided during the analysis.

---

# 2. Comparison of LLM Output and Workbook

| Strategy | LLM Result | Workbook Result | Match? | Diagnosis |
|----------|-----------:|----------------:|:------:|-----------|
| Forward Hedge | $14,607,500 | $14,607,500 | ✅ | Independent LLM calculation matched the workbook. |
| Money Market Hedge | $14,607,488 | $14,607,488 | ✅ | Independent LLM calculation matched the workbook. |
| Put Hedge (S_T = -5%) | $14,190,000 | $14,190,000 | ✅ | Independent LLM calculation matched the workbook. |
| Put Hedge (S_T = Current Spot) | $14,190,000 | $14,190,000 | ✅ | Independent LLM calculation matched the workbook. |
| Put Hedge (S_T = +5%) | $14,910,125 | $14,910,125 | ✅ | Independent LLM calculation matched the workbook. |
| Call Comparison (S_T = +5%) | $445,125 | $445,125 | ✅ | Independent LLM calculation matched the workbook. |

The independently generated LLM analysis produced results consistent with the completed workbook. No material discrepancies were identified. This indicates that the Stage 2 specification and Stage 4 market-data documentation contained sufficient information for another model to independently reproduce the hedge calculations without additional coaching.

---

# 3. Hand Verification

The following calculations were independently verified by hand using the named-range values from the completed workbook. These calculations were performed with a calculator and compared against the workbook outputs.

## A. Forward Hedge

Formula:

```
Forward Proceeds = FC_AMT × F0_in
```

Substitution:

```
12,500,000 × 1.1686
```

Calculation:

```
= 14,607,500
```

**Verified Result:** **$14,607,500**

This matches the workbook exactly.

---

## B. Money-Market Hedge

### Step 1 – Borrow EUR Today

Formula:

```
Borrow EUR = FC_AMT ÷ (1 + R_FC × T_DAYS ÷ 360)
```

Substitution:

```
12,500,000 ÷ (1 + 0.0258)
```

Calculation:

```
= 12,185,611 EUR
```

---

### Step 2 – Convert Borrowed EUR to USD

Formula:

```
USD Today = Borrowed EUR × S0_in
```

Substitution:

```
12,185,611 × 1.1522
```

Calculation:

```
= $14,040,261
```

---

### Step 3 – Invest USD

Formula:

```
USD at Maturity = USD Today × (1 + R_USD)
```

Substitution:

```
14,040,261 × 1.0404
```

Calculation:

```
= $14,607,488
```

**Verified Result:** **$14,607,488**

This matches the workbook.

---

## C. Put Option Hedge (S_T = 1.2098)

Gross proceeds:

```
12,500,000 × 1.2098
= $15,122,500
```

Premium:

```
12,500,000 × 0.017
= $212,500
```

Net proceeds:

```
15,122,500 − 212,500
= $14,910,000
```

**Workbook Result:** **$14,910,125**

The small difference is due to the workbook using the full-precision ending spot rate (approximately 1.20981) rather than the rounded value shown in the sensitivity table. Using the full precision reproduces the workbook result exactly.

---

The manually calculated results agree with the completed workbook, confirming that the formulas, named ranges, and hedge calculations were implemented correctly.

---

# 4. Specification Retrospective

The independent LLM analysis demonstrated that the Stage 2 specification and Stage 4 market-data memo were detailed enough to reproduce the hedge calculations without access to the workbook. The independently generated results closely matched the completed workbook, indicating that the specification successfully communicated the required model structure, formulas, and assumptions.

The validation process also identified several areas where the specification was strengthened during the project. One important revision involved the euro interest rate source. The original specification referenced the European Central Bank as the source for `R_FC`. During Stage 4, I recognized that the ECB policy rate is an overnight rate and does not match the one-year hedge horizon. I revised the model to use the one-year German government bond yield so that both currencies used consistent one-year market rates. This improved the realism of the Covered Interest Parity calculations.

Another improvement involved the validation rules. The original build used a very loose tolerance for comparing the Forward Hedge and Money Market Hedge proceeds. After reviewing instructor feedback, I revised the specification to require a much tighter validation tolerance. This made the workbook's PASS/REVIEW checks more meaningful and better aligned with the expected precision of the model.

The project also highlighted the importance of writing specifications that remove ambiguity. The revised specification now clearly requires formula-driven sensitivity analysis, formula-only calculated outputs, explicit validation rules, and consistency between the Option Hedge worksheet and the Sensitivity Analysis worksheet. These additions reduced opportunities for interpretation during the workbook build.

If I were creating a second version of the specification, I would make the treatment of validation tolerances, scenario assumptions, and illustrative option scenarios even more explicit. I would also clearly distinguish between indicative placeholder values used during development and live market data used for the final analysis. These improvements would make the specification even easier for another analyst or AI system to implement without additional clarification.
