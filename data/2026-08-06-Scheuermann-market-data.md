# Stage 4 Market Data Memo

**Course:** FIN 321 – International Business Finance  
**Student:** Ella Scheuermann  
**Date:** August 6, 2026

## Purpose

This memo documents the live market inputs used to replace the Stage 2 placeholder values. Unless otherwise noted, all market data was retrieved on August 6, 2026 at approximately 3:30 PM HST.

## Market Data

| Input | Value | Source | Retrieval Time | Notes |
|-------|------:|--------|----------------|------|
| FC_AMT | €12,500,000 | Instructor scenario | N/A | Scenario value |
| S0_in | 1.1522 USD/EUR | Investing.com | Aug. 6, 2026 – 3:30 PM HST | Live EUR/USD spot rate |
| R_USD | 4.04% | U.S. Department of the Treasury | Aug. 6, 2026 – 3:30 PM HST | One-year U.S. Treasury yield selected because it matches the one-year hedge horizon. |
| R_FC | 2.58% | Trading Economics | Aug. 6, 2026 – 3:30 PM HST | One-year German government bond yield used as a euro-denominated sovereign proxy. |
| F0_in | 1.1686 USD/EUR | Computed | Aug. 6, 2026 | Covered Interest Parity (CIP) implied forward rate calculated from S0_in, R_USD, and R_FC. |
| K_PUT | 1.1522 USD/EUR | Set equal to live spot | Aug. 6, 2026 | At-the-money strike following the scenario convention. |
| K_CALL | 1.1522 USD/EUR | Set equal to live spot | Aug. 6, 2026 | At-the-money strike following the scenario convention. |
| PREM_PUT | 0.017 | Instructor scenario | N/A | Scenario premium retained as instructed. |
| PREM_CALL | 0.022 | Instructor scenario | N/A | Scenario premium retained as instructed. |
| T_DAYS | 360 | Instructor scenario | N/A | ACT/360 convention. |

---

## CIP-Implied Forward Rate

Because a reliable live one-year EUR/USD forward quote was not used, the forward rate was calculated using Covered Interest Parity (CIP):

F₀ = S₀ × (1 + R_USD × T / 360) / (1 + R_FC × T / 360)

Using:

- S₀ = 1.1522
- R_USD = 4.04%
- R_FC = 2.58%
- T = 360 days

Result:

**F₀ = 1.1686 USD/EUR**

The Stage 2 indicative forward rate was **1.16924 USD/EUR**. The live CIP-implied forward differs by approximately **0.00064**, which is a very small difference and is consistent with changes in market conditions since the original scenario was created.

---

## Assumptions

- Option premiums remain the instructor-provided scenario values because reliable retail FX option quotes are difficult to obtain.
- FC_AMT and T_DAYS remain fixed according to the assigned scenario.
- No transaction costs, taxes, brokerage fees, or bid-ask spreads are included.

---

## Workbook Population

The live values were entered only into the named-range input cells. No formulas or worksheet structure were modified during population.

---

## FX Hedging Lab Cross-Check

The live market inputs were entered into the FX Hedging Lab using:

- Spot rate (S0_in): 1.1522 USD/EUR
- Forward rate (F0_in): 1.1686 USD/EUR (CIP-implied)
- USD interest rate (R_USD): 4.04%
- EUR interest rate (R_FC): 2.58%
- Foreign currency receivable (FC_AMT): €12,500,000
- Days to settlement (T_DAYS): 360
- Put strike (K_PUT): 1.1522
- Call strike (K_CALL): 1.1522
- Put premium (PREM_PUT): 0.017
- Call premium (PREM_CALL): 0.022

The FX Hedging Lab produced a forward hedge value of approximately $14,607,500 and a money market hedge value of approximately $14,607,488. The $12 difference is attributable to rounding, and the lab confirmed that covered interest parity holds. These results were consistent with my workbook, and no structural discrepancies were identified.
