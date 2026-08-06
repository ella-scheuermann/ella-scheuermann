## 1. Problem Statement

The U.S. technology services firm expects to receive a €12,500,000 receivable from a European customer at settlement in one year. Because the payment is denominated in euros, the U.S. dollar value of the receivable will depend on the EUR/USD exchange rate on the settlement date.

If the euro depreciates before settlement, the company will receive fewer U.S. dollars than expected. This would reduce cash inflows, create budgeting uncertainty, and increase foreign exchange risk. The purpose of this workbook is to evaluate the unhedged position alongside three hedging strategies—a forward contract, a money-market hedge, and a currency option—to determine which approach best protects the company's U.S. dollar proceeds.

The workbook will be designed so that all market inputs are placeholders (indicative values) during this phase and can be replaced with live market data during Phase 4 without changing the workbook's structure or formulas.

## 2. Inputs — Named-Range Contract

The workbook will use the ten required named ranges listed below. These values are **indicative inputs for Phase 2 and will be verified or replaced with live market data in Phase 4**. All workbook formulas must reference the named ranges rather than cell addresses.

| Named Range | Description | Placeholder Value | Unit | Phase 4 Data Source |
|---|---|---:|---|---|
| `FC_AMT` | Foreign-currency receivable | 12,500,000 | EUR | Instructor-assigned scenario |
| `S0_in` | Spot rate at inception | 1.1522 | USD per EUR | Live EUR/USD spot quote |
| `F0_in` | One-year forward rate | 1.16924 | USD per EUR | Live one-year EUR/USD forward quote or covered-interest-parity calculation |
| `R_USD` | U.S. annual interest rate | 4.059% | Annual % | U.S. one-year Treasury yield |
| `R_FC` | Euro annual interest rate | 2.00% indicative placeholder | Annual % | One-year euro-area government yield or ECB reference rate |
| `K_PUT` | Put option strike | 1.1522 | USD per EUR | Set at or near the verified Phase 4 spot rate |
| `K_CALL` | Call option strike | 1.1522 | USD per EUR | Set at or near the verified Phase 4 spot rate |
| `PREM_PUT` | Put premium per euro | 0.017 | USD per EUR | Instructor-provided scenario assumption |
| `PREM_CALL` | Call premium per euro | 0.022 | USD per EUR | Instructor-provided scenario assumption |
| `T_DAYS` | Days to settlement | 360 | Days | One-year scenario using ACT/360 |
