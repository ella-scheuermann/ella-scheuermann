# Build Audit – Phase 3

**Course:** FIN 321 – International Business Finance  
**Student:** Ella Scheuermann  
**Date:** August 6, 2026  
**Workbook:** `2026-08-06-scheuermann-tech-services-model.xlsx`

---

## Audit Objective

Review the completed workbook against the Phase 2 specification to verify that calculations, named ranges, formulas, and validation checks function correctly.

---

## Findings

### Finding 1 – Named Ranges

**Test Performed:**
Opened **Formulas → Name Manager** and verified that the required named ranges exist.

**Result:**
PASS

**Notes:**
The workbook includes the required named ranges, including:

- `FC_AMT`
- `S0_in`
- `F0_in`
- `R_USD`
- `R_FC`
- `K_PUT`
- `K_CALL`
- `PREM_PUT`
- `PREM_CALL`
- `T_DAYS`

---

### Finding 2 – Formula Integrity

**Test Performed:**
Reviewed calculation worksheets to verify that calculated values are generated using Excel formulas rather than manually entered numbers.

**Result:**
PASS

**Notes:**
Key calculations use formulas throughout the workbook, including the Forward Hedge, Money Market Hedge, Option Hedge, and Sensitivity Analysis worksheets.

---

### Finding 3 – Sensitivity Analysis

**Test Performed:**
Changed the value of the named input `S0_in` and confirmed that the sensitivity table recalculated automatically.

**Result:**
PASS

**Notes:**
The sensitivity table references `S0_in`, allowing all scenarios to update automatically whenever the spot exchange rate changes.

---

### Finding 4 – Specification Validation Checks

**Test Performed:**
Verified that the workbook's internal validation checks from the Phase 2 specification operate correctly.

**Result:**
PASS

**Notes:**
The workbook includes validation checks for model consistency, including forward pricing, money market calculations, and scenario analysis. No errors were identified during testing.

---

### Finding 5 – Minor Recommendation

**Observation:**
The Option Hedge worksheet uses a manually entered future spot rate (`S_T`) for scenario analysis.

**Notes:**
This is appropriate for evaluating different exchange rate outcomes.

---

## Overall Assessment

The workbook successfully meets the Phase 3 build requirements. Required named ranges are present, calculations are implemented using formulas, the sensitivity analysis updates dynamically when the spot rate input changes, and the validation checks pass. No material issues were identified during the audit. 
