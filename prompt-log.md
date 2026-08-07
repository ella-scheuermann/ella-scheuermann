# AI Prompt Log

This file documents meaningful uses of AI throughout my portfolio projects.

## 2026-07-24 — Portfolio Setup

**Tool:** ChatGPT

**Prompt:**
Help me create my GitHub portfolio repository, write my bio, and organize the required folder structure.

**How I used it:**
I reviewed and edited all AI-generated content before adding it to my repository.

---

## 2026-08-06 — Stage 2: FX Hedging Model Specification

**Tool:** ChatGPT (GPT-5.5)

**Prompt:**
Help me draft the Stage 2 FX Hedging Model Specification using the course template. Follow the required eight-section structure, use the required named ranges, and explain each section step by step so I understand both the financial concepts and how to write the specification.

**Draft → Revision Example:**
The initial AI draft included the required sections but did not consistently identify the market inputs as **"indicative – replaced with live market data at Stage 4."** I revised the specification by adding this language throughout the inputs table and assumptions section. I also refined the calculation flow to use only named-range notation, expanded the validation rules, clarified the sensitivity analysis requirements, and improved the wording to better match the course instructions and rubric.

**How I used it:**
ChatGPT helped draft the initial specification and explain the technical concepts. I reviewed every section, edited the wording, made corrections, added missing details required by the rubric, and ensured the final document accurately reflected my understanding before committing it to my GitHub repository.

---

## 2026-08-06 — Stage 3: AI-Assisted Workbook Build

**Tool:** Claude

**Prompt:**
I have attached my committed Stage 2 FX Hedging Model Specification. Treat that document as the complete and binding build contract. Build the Excel workbook exactly as specified. Do not simplify, reinterpret, omit, or replace any requirement. Do not re-explain the model to me in chat. If the specification contains a genuine ambiguity that prevents a correct build, ask me a concise clarification question before generating the workbook.

Important correction to the attached specification: Use `R_FC = 2.576%` as the indicative annual euro interest rate. This replaces the TBD placeholder in the spec.

Required output: Create and return one downloadable Microsoft Excel workbook in `.xlsx` format. Do not return a CSV, PDF, Word file, Markdown table, or instructions for me to build it manually.

Required filename: `2026-08-06-Scheuermann-tech-services-model.xlsx`

The workbook must satisfy every item below.

1. **Exact named-range contract**

Create all ten named ranges exactly as written and attach each one to the correct input cell:

`FC_AMT`  
`S0_in`  
`F0_in`  
`R_USD`  
`R_FC`  
`K_PUT`  
`K_CALL`  
`PREM_PUT`  
`PREM_CALL`  
`T_DAYS`

Use these indicative Stage 2 values:

`FC_AMT = 12500000`  
`S0_in = 1.1522`  
`F0_in = 1.16924`  
`R_USD = 4.059%`  
`R_FC = 2.576%`  
`K_PUT = 1.1522`  
`K_CALL = 1.1522`  
`PREM_PUT = 0.017`  
`PREM_CALL = 0.022`  
`T_DAYS = 360`

Clearly label all market values: “Indicative — replaced with live market data at Stage 4.”

2. **Required worksheets**

Create these worksheets in this order:

Cover  
Legend-Key Inputs  
Forward Hedge  
Money Market Hedge  
Option Hedge  
Sensitivity Analysis  
Notes-Assumptions

3. **Cover worksheet**

Include:

Project title  
Scenario: U.S. Tech Services Firm  
Exposure: EUR 12,500,000 receivable  
Settlement horizon: one year  
Author: Ella Scheuermann  
Build date  
Workbook version  
A data-provenance block explaining that market inputs are indicative placeholders and will be replaced with live, sourced market data at Stage 4

4. **Legend and formatting**

Create a visible legend and apply this color convention consistently throughout the workbook:

Yellow = inputs  
Blue = assumptions  
Green = formulas  
Gray = outputs / KPIs

Use professional formatting, readable column widths, clear section headings, currency and percentage number formats, and freeze panes where useful.

5. **Inputs worksheet**

Show all ten named inputs in a table with:

Named range  
Description  
Value  
Unit  
Stage 4 data source  
Placeholder status

Input cells must be editable and colored yellow.

Use these source labels:

`FC_AMT`: Instructor-assigned scenario  
`S0_in`: Yahoo Finance EUR/USD  
`F0_in`: One-year forward FX quote  
`R_USD`: U.S. Treasury one-year yield  
`R_FC`: European one-year reference rate / government yield  
`K_PUT`: Option market quote, set at or near spot  
`K_CALL`: Option market quote, set at or near spot  
`PREM_PUT`: Instructor scenario / option market  
`PREM_CALL`: Instructor scenario / option market  
`T_DAYS`: Contract terms, ACT/360 convention

6. **Forward Hedge worksheet**

Calculate:

`Forward USD Proceeds = FC_AMT × F0_in`

The calculated result must be a formula, not a typed value. Display the formula logic, the result, and a short note that the proceeds remain fixed regardless of `S_T`.

7. **Money Market Hedge worksheet**

Show the three required steps as separate formula cells:

Step 1 — Borrow EUR today:  
`FC_AMT / (1 + R_FC × T_DAYS / 360)`

Step 2 — Convert borrowed EUR to USD:  
`Borrowed EUR × S0_in`

Step 3 — Invest USD:  
`USD at inception × (1 + R_USD × T_DAYS / 360)`

Also calculate:

`Implied forward rate = S0_in × (1 + R_USD × T_DAYS / 360) / (1 + R_FC × T_DAYS / 360)`

Include visible checks for:

Difference between implied forward rate and `F0_in`  
Difference between forward proceeds and money-market proceeds  
PASS / REVIEW status based on a clearly stated rounding tolerance

All calculations must be formulas.

8. **Option Hedge worksheet**

Include a clearly labeled `S_T` input or scenario value used only for payoff illustration.

Put calculations:

`Gross put proceeds = FC_AMT × MAX(S_T, K_PUT)`  
`Total put premium = FC_AMT × PREM_PUT`  
`Net put proceeds = FC_AMT × MAX(S_T, K_PUT) − FC_AMT × PREM_PUT`

Call comparison calculations:

`Gross call payoff = FC_AMT × MAX(S_T − K_CALL, 0)`  
`Total call premium = FC_AMT × PREM_CALL`  
`Net call payoff = FC_AMT × MAX(S_T − K_CALL, 0) − FC_AMT × PREM_CALL`

Clearly state that the put is the primary hedge for a EUR receivable and the call is included as a comparison case.

9. **Sensitivity Analysis worksheet**

Create exactly 11 ending spot-rate scenarios from `0.95 × S0_in` through `1.05 × S0_in` in 1% increments.

Do not hand-type the 11 resulting spot-rate values. Build the rows with formulas linked to `S0_in`.

For each scenario, calculate with formulas:

Unhedged USD proceeds  
Forward hedge USD proceeds  
Money-market hedge USD proceeds  
Put-option net proceeds

Create one line chart with:

X-axis: Ending Spot Rate, `S_T`  
Y-axis: USD Proceeds

Series:

Unhedged  
Forward Hedge  
Money Market Hedge  
Put Option Hedge

The chart must update automatically when an input changes.

10. **Notes and Assumptions worksheet**

Document:

ACT/360 day-count convention  
Transaction costs, taxes, brokerage fees, and bid-ask spreads assumed to be zero  
Covered interest rate parity expectation  
Option premiums treated as costs and deducted in every scenario  
Premiums are not compounded  
Exchange rates quoted as USD per EUR  
All market values are indicative and replaced with live data at Stage 4  
Named ranges are the required modeling vocabulary

11. **Formula-only requirement**

Every calculated cell must contain an Excel formula.

Do not hardcode:

Forward proceeds  
Money-market steps  
Parity results  
Option proceeds  
Sensitivity values  
Validation differences  
PASS / REVIEW checks

Hardcoded input values are allowed only in the designated input cells.

Use named ranges in formulas wherever the formula depends directly on one of the ten required inputs. Intermediate calculated cells may be referenced by their own descriptive named ranges if helpful.

12. **Validation requirements**

Create a visible validation area in the workbook that checks:

All ten required named ranges exist  
Forward and money-market proceeds approximately agree  
Implied forward approximately agrees with `F0_in`  
No formula error cells  
Every output is formula-driven  
Sensitivity table contains 11 scenarios  
Sensitivity values recalculate when inputs change  
Put proceeds are continuous at `K_PUT`

Each validation item should visibly display PASS or REVIEW.

13. **Final self-audit before delivery**

Before returning the file, inspect the workbook and confirm:

All required worksheets exist  
All ten required named ranges exist and point to the intended input cells  
Every calculated result is a formula  
No required output is hardcoded  
All hedge calculations are present  
The money-market hedge is shown in three separate steps  
The sensitivity table contains 11 formula-driven rows  
The chart exists and uses the sensitivity table  
The parity checks are visible  
The color convention is applied consistently  
No Excel error values are present

Do not tell me that the workbook is complete unless you have checked these items. Return the completed downloadable `.xlsx` file only.

**Draft → Revision Example:**
I used ChatGPT to help create and edit the prompt before submitting it to Claude. The original request was shorter and did not fully specify the workbook structure, formula-only requirements, validation checks, named-range contract, sensitivity analysis, or final self-audit. I revised the prompt by adding these detailed requirements and by correcting the euro interest rate to `R_FC = 2.576%`.

**How I used it:**
Claude generated the first version of the Excel workbook from my committed Stage 2 specification. I downloaded the workbook, reviewed it manually in Excel, tested the named ranges, checked that calculated cells used formulas, changed `S0_in` to confirm that the sensitivity table updated, and documented the results in a separate build audit. I used ChatGPT to help develop and edit the prompt, but I reviewed the workbook and completed the final audit myself.

---
## 2026-08-06 — Stage 4: Market Data Population

**Tool:** ChatGPT (GPT-5.5)

**Prompt:**
Help me organize the Stage 4 market-data memo, cross-reference my retrieved market values, calculate the Covered Interest Parity (CIP) implied forward rate, and verify that my workbook outputs matched the FX Hedging Lab.

**Draft → Revision Example:**
The initial AI draft provided a basic structure for the market-data memo. I revised it by adding my own retrieved market values, documenting the sources and retrieval timestamps, including the rationale for my interest rate selections, and incorporating the results of the FX Hedging Lab cross-check.

**How I used it:**
ChatGPT helped create the initial draft of the market-data memo, cross-reference my retrieved market data, calculate the CIP-implied forward rate, and organize the required documentation. I independently located the market data, verified the sources, entered the live values into my workbook, and confirmed the results using the FX Hedging Lab.

---

*This log will be updated throughout the semester.*
