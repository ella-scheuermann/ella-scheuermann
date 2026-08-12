## 1. Problem Statement

The U.S. technology services firm expects to receive a €12,500,000 receivable from a European customer at settlement in one year. Because the payment is denominated in euros, the U.S. dollar value of the receivable will depend on the EUR/USD exchange rate on the settlement date.

If the euro depreciates before settlement, the company will receive fewer U.S. dollars than expected. This would reduce cash inflows, create budgeting uncertainty, and increase foreign exchange risk. The purpose of this workbook is to evaluate the unhedged position alongside three hedging strategies—a forward contract, a money-market hedge, and a currency option—to determine which approach best protects the company's U.S. dollar proceeds.

The workbook will be designed so that all market inputs are placeholders (indicative values) during this phase and can be replaced with live market data during Stage 4 without changing the workbook's structure or formulas. This specification is intended to provide enough detail for an AI system or another analyst to build the complete workbook without requiring additional clarification.

## 2. Inputs (Named-Range Contract)

The workbook will use the following named ranges as required inputs. **Unless otherwise noted, all market values listed below are indicative and will be replaced with live market data during Stage 4.** The workbook will reference these named ranges throughout the model rather than individual cell addresses.

| Named Range | Description | Placeholder Value | Unit | Stage 4 Data Source |
|-------------|-------------|------------------|------|---------------------|
| FC_AMT | Foreign-currency receivable | €12,500,000 | EUR | Instructor-assigned scenario |
| S0_in | Spot exchange rate | **1.1522 (indicative – replaced with live market data at Stage 4)** | USD per EUR | Yahoo Finance (EUR/USD) |
| F0_in | One-year forward exchange rate | **1.16924 (indicative – replaced with live market data at Stage 4)** | USD per EUR | Forward FX market quote |
| R_USD | U.S. annual interest rate | **4.059% (indicative – replaced with live market data at Stage 4)** | Annual % | U.S. Treasury 1-Year Yield |
| R_FC | Euro annual interest rate | **2.576 (indicative – replaced with live market data at Stage 4)** | Annual % | European Central Bank |
| K_PUT | Put option strike price | **1.1522 (indicative – replaced with live market data at Stage 4)** | USD per EUR | Option market quote |
| K_CALL | Call option strike price | **1.1522 (indicative – replaced with live market data at Stage 4)** | USD per EUR | Option market quote |
| PREM_PUT | Put option premium | **0.017 (indicative – replaced with live market data at Stage 4)** | USD per EUR | Instructor scenario / option market |
| PREM_CALL | Call option premium | **0.022 (indicative – replaced with live market data at Stage 4)** | USD per EUR | Instructor scenario / option market |
| T_DAYS | Days to settlement | 360 | Days | Contract terms (ACT/360 convention) |

## 3. Tab Architecture

The workbook will be organized into separate worksheets so that inputs, calculations, assumptions, and outputs are clearly separated. Each worksheet has a specific purpose to improve readability, simplify auditing, and allow future updates without affecting the overall workbook structure.

| Worksheet | Purpose |
|------------|---------|
| Cover | Provides the project title, scenario summary, author, version information, and instructions for using the workbook. |
| Legend/Key | Explains the workbook color scheme (inputs, formulas, assumptions, and outputs) and documents the named-range conventions used throughout the workbook. |
| Inputs | Contains all user-editable inputs, including exchange rates, interest rates, option premiums, settlement period, and the foreign-currency receivable. All inputs will use the named ranges defined in Section 2. |
| Forward Hedge | Calculates the USD proceeds from a forward contract using the quoted forward exchange rate. |
| Money Market Hedge | Calculates the money-market hedge using the three-step borrow, convert, and invest process. Includes the covered interest parity check. |
| Option Hedge | Calculates the payoff from the put option and includes the call option as a comparison case. Displays both gross and net proceeds after premiums. |
| Sensitivity Analysis | Compares unhedged, forward, money-market, and option strategies over a range of possible ending exchange rates from 95% to 105% of the initial spot rate. Includes a comparison chart. |
| Notes & Assumptions | Documents assumptions, data sources, conventions, and any modeling limitations used throughout the workbook. |

## 4. Assumptions & Constraints

The workbook will be developed using the following assumptions and modeling conventions:

- Interest calculations will follow the **ACT/360** day-count convention using **T_DAYS = 360**.
- All market inputs shown in Stage 2 are **indicative and will be replaced with live market data during Stage 4**.
- Transaction costs, brokerage commissions, taxes, and bid-ask spreads are assumed to be zero unless otherwise specified.
- Covered Interest Rate Parity (CIRP) is assumed to hold. Therefore, the forward hedge and money-market hedge should produce nearly identical results after accounting for normal rounding differences.
- Option premiums are treated as upfront costs and will always be deducted from total proceeds regardless of whether the option is exercised.
- Exchange rates are expressed in **USD per EUR** throughout the workbook.
- All calculations will reference named ranges rather than individual Excel cell addresses to improve readability, consistency, and auditability.


## 5. Calculation Flow

All calculations must use the named ranges defined in Section 2. No formulas may use Excel cell addresses. Each major calculation must appear as a separate, visible Excel formula so that the workbook can be audited easily. Every calculated value in the workbook—including intermediate calculations, validation checks, PASS/REVIEW indicators, chart source data, sensitivity tables, and summary outputs—must be generated by an Excel formula. No calculated value may be entered as a typed number, pasted result, or manually copied output. Only the designated named-range input cells may contain hardcoded values.

`S_T` represents the ending EUR/USD spot rate at settlement. It is a scenario variable used in the option and sensitivity calculations rather than one of the ten required input named ranges.

### 5.1 Unhedged Position

The unhedged position shows the USD value of the receivable if the euros are converted at the ending spot rate.

**Unhedged USD proceeds:**

`FC_AMT × S_T`

The unhedged result increases when the euro strengthens and decreases when the euro weakens.

### 5.2 Forward Hedge

The forward hedge locks in the quoted forward exchange rate for settlement.

**Forward USD proceeds:**

`FC_AMT × F0_in`

The forward proceeds must remain constant across all ending spot-rate scenarios because the exchange rate is fixed at inception.

### 5.3 Money-Market Hedge

The money-market hedge must display the following three steps separately.

**Step 1 — Borrow foreign currency today**

`FC_AMT / (1 + R_FC × T_DAYS / 360)`

This calculation determines the amount of EUR to borrow today so that the €12,500,000 receivable will repay the foreign-currency loan exactly at settlement.

**Step 2 — Convert the borrowed EUR into USD**

`Foreign-currency borrowing amount × S0_in`

This converts the EUR borrowed in Step 1 into U.S. dollars at the inception spot rate.

**Step 3 — Invest the USD proceeds**

`USD amount at inception × (1 + R_USD × T_DAYS / 360)`

The result of Step 3 is the total USD proceeds from the money-market hedge at settlement.

### 5.4 Covered Interest Rate Parity Check

Calculate the forward rate implied by the inception spot rate and the two interest rates:

`F_implied = S0_in × (1 + R_USD × T_DAYS / 360) / (1 + R_FC × T_DAYS / 360)`

The implied forward rate should approximately equal `F0_in`.

The workbook must also compare:

`FC_AMT × F0_in`

with the money-market proceeds from Step 3. These results should be approximately equal, allowing only for minor rounding differences.

### 5.5 Put Option Hedge

The put option establishes a minimum conversion rate while allowing the firm to benefit if the euro strengthens.

**Gross put-option proceeds:**

`FC_AMT × MAX(S_T, K_PUT)`

**Total put premium:**

`FC_AMT × PREM_PUT`

**Net put-option proceeds:**

`FC_AMT × MAX(S_T, K_PUT) − FC_AMT × PREM_PUT`

When `S_T` is below `K_PUT`, the strike price creates a floor under the USD proceeds. When `S_T` is above `K_PUT`, the put expires unexercised and the company converts the receivable at the more favorable market rate. The put premium is deducted in every scenario and is not compounded.

### 5.6 Call Option Comparison

The workbook must also show the call option’s participation payoff as a comparison case.

**Gross call payoff:**

`FC_AMT × MAX(S_T − K_CALL, 0)`

**Total call premium:**

`FC_AMT × PREM_CALL`

**Net call payoff:**

`FC_AMT × MAX(S_T − K_CALL, 0) − FC_AMT × PREM_CALL`

The call must be clearly identified as a comparison case rather than the primary hedge for a EUR receivable because a stronger euro already increases the USD value of the receivable.

## 6. Sensitivity Plan

The workbook will include a sensitivity analysis to evaluate how changes in the ending EUR/USD exchange rate affect the USD proceeds under each hedging strategy. The ending spot rate (`S_T`) will vary from **0.95 × S0_in** to **1.05 × S0_in** in **1% increments**, producing a total of **11 exchange-rate scenarios**.

For each scenario, the workbook will calculate:

- Unhedged USD proceeds
- Forward hedge proceeds
- Money-market hedge proceeds
- Put-option hedge proceeds

Every cell in the sensitivity table must contain an Excel formula. The ending spot-rate scenarios, percentage changes, hedge proceeds, comparison values, and chart source data must all be formula-driven. No calculated sensitivity value may be manually entered, copied, or hardcoded. Changing S0_in must automatically recalculate all eleven scenarios, every proceeds value, and the comparison chart without any manual edits. Each of the eleven scenario rows must calculate independently using Excel formulas. No scenario row may consist of copied values, manually entered results, or values pasted from another calculation.

### Comparison Chart

The comparison chart will display:

- X-axis: Ending spot exchange rate (`S_T`)
- Y-axis: USD proceeds

The chart will contain one line for each strategy:

- Unhedged
- Forward Hedge
- Money-Market Hedge
- Put Option Hedge

The purpose of the chart is to allow the Chief Financial Officer to quickly compare how each hedging strategy performs under changing exchange-rate conditions, identify the downside protection provided by each strategy, and understand the trade-off between exchange-rate certainty and upside participation.

## 7. Validation Rules (Check Figures)

The completed workbook must satisfy the following validation checks before it is considered complete.

1. **Covered Interest Rate Parity**

   The implied forward rate must approximately equal `F0_in`, and the forward hedge proceeds must approximately equal the money-market hedge proceeds, allowing only for minor rounding differences.

2. **Formula Integrity**

   Every calculated output, intermediate calculation, validation result, PASS/REVIEW indicator, chart source value, and every row of the sensitivity analysis must be generated by an Excel formula. No calculated cell anywhere in the workbook may contain a manually entered number or pasted result. The workbook must fully recalculate whenever any named-range input changes.
   
4. **Named-Range Consistency**

   All workbook calculations must reference the named ranges defined in Section 2 rather than Excel cell addresses. Named ranges are the required modeling vocabulary throughout the workbook.
   
4. **Error-Free Workbook**

   The workbook must not contain any `#DIV/0!`, `#VALUE!`, `#REF!`, `#NAME?`, or other Excel error messages.

5. **Option Payoff Validation**

   The put-option payoff must remain continuous across all ending exchange-rate scenarios. Below the strike price, the payoff floor should be maintained. Above the strike price, the option should expire unexercised and the proceeds should follow the market exchange rate, less the premium.

6. **Sensitivity Analysis Validation**

   The sensitivity table must calculate all eleven exchange-rate scenarios from **0.95 × S0_in** through **1.05 × S0_in** in **1% increments**, and the comparison chart must update automatically whenever an input value changes.

7. **Hardcoded Output Validation**

   The completed workbook must be inspected before delivery to verify that all calculated cells contain Excel formulas. No calculated outputs, intermediate results, sensitivity values, chart source data, or validation results may be stored as manually entered numbers. Changing any named-range input must automatically recalculate every dependent worksheet without requiring manual edits.


## 8. Outputs

The completed workbook will present the following outputs to summarize the performance of each hedging strategy. All output cells will be formula-driven (gray cells) and will update automatically whenever an input value changes.

### Summary Outputs

The workbook will display:

- Unhedged USD proceeds
- Forward hedge USD proceeds
- Money-market hedge USD proceeds
- Put-option hedge USD proceeds
- Call-option comparison payoff
- Implied forward rate (Covered Interest Rate Parity)
- Difference between the implied forward rate and the quoted forward rate
- Difference between forward hedge proceeds and money-market hedge proceeds

### Summary Tables

The workbook will include the following tables:

1. **Input Summary Table**
   - Lists all named input values used in the analysis.

2. **Hedge Comparison Table**
   - Compares the unhedged position, forward hedge, money-market hedge, and put-option hedge using the same market assumptions.

3. **Sensitivity Analysis Table**
   - Displays USD proceeds for each hedging strategy over eleven ending exchange-rate scenarios ranging from **0.95 × S0_in** to **1.05 × S0_in** in 1% increments.

### Charts

The workbook will include one comparison line chart displaying the USD proceeds of each strategy across all sensitivity scenarios. The chart will allow users to compare the trade-offs between downside protection, exchange-rate certainty, and upside participation.

### Workbook Outputs

All summary outputs and tables will update automatically whenever an input value changes. Every output, intermediate calculation, validation result, and sensitivity-analysis value will be generated by Excel formulas. All calculations will update automatically whenever any named-range input changes. No calculated output, comparison value, chart source, or validation result may be hardcoded, pasted as a value, or manually edited after calculation.
