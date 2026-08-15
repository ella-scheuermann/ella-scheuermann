# Stage 5 review — tech services LLM analysis & validation · Treasury sign-off

Ella — the best thing in this submission happened back in Stage 4, and your retrospective is where you finally get credit for it:

> *"The original specification referenced the European Central Bank as the source for `R_FC`. During Stage 4, I recognized that the ECB policy rate is an overnight rate and does not match the one-year hedge horizon. I revised the model to use the one-year German government bond yield so that both currencies used consistent one-year market rates."*

A classmate found this exact error — an overnight euro rate paired with a 365-day horizon and a 1-year Treasury on the dollar leg — but found it *after* Stage 5, quantified the damage at $12,629, and never re-ran the model. You caught it a stage earlier, on your own, without a reviewer pointing at it, and you propagated the fix so that everything downstream is built on a tenor-matched rate. That is the difference between documenting a defect and not having one.

You also tightened the parity tolerance after the Stage 3 review rather than leaving a check that passed everything: *"This made the workbook's PASS/REVIEW checks more meaningful."* Both changes show the loop working the way it is supposed to.

| Criterion | Score |
|---|---|
| LLM execution & comparison | 25 / 25 |
| Hand verification | 25 / 25 |
| Recommendation & executive voice | 25 / 25 |
| Spec retrospective | 17 / 17 |
| Repo polish | 4.8 / 8 |
| **Total** | **97 / 100** |

**What you did well — and why it matters**

- **Your numbers reconcile, including the one that did not.** `12,500,000 / 1.0258 = €12,185,611` ✓ → `× 1.1522 = $14,040,261` ✓ → `× 1.0404 = $14,607,488` ✓, against a forward of `12,500,000 × 1.1686 = $14,607,500` ✓. The $12 gap is exactly what a four-decimal `F0_in` produces — the CIP value from your own rates is `1.1522 × 1.0404 / 1.0258 = 1.168599`.
- **You chased down a $125 difference instead of waving at it.** *"The small difference is due to the workbook using the full-precision ending spot rate (approximately 1.20981) rather than the rounded value shown in the sensitivity table."* Correct — `12,500,000 × 1.20981 − 212,500 = $14,910,125` ✓. Most people would have written "rounding" and moved on; you identified *which* rounding, and where.
- **You distinguished placeholder from live data as a spec principle.** *"I would also clearly distinguish between indicative placeholder values used during development and live market data used for the final analysis."* Two classmates had LLM runs silently reuse Stage 2 placeholder numbers and produce badly wrong grids. You identified the hazard without having to be burned by it.

**The one substantive correction — a validation that finds nothing is a statement about the test**

Your comparison table has six rows and six matches, and the conclusion drawn is: *"No material discrepancies were identified. This indicates that the Stage 2 specification and Stage 4 market-data documentation contained sufficient information for another model to independently reproduce the hedge calculations."*

That conclusion is only as strong as the test behind it, and this test is narrow. Consider what it does **not** cover:

- **No unhedged row at all.** The unhedged position is the one strategy that moves with `S_T` at every point, and it is where two of your classmates' LLMs went wrong — both silently rebuilt the sensitivity grid off a stale Stage 2 spot and produced no-hedge proceeds that were off by half a million dollars. Your table cannot detect that failure, because it never asks the question.
- **Forward and money market appear at a single point each.** Both are `S_T`-invariant, so one row is genuinely enough — but that means four of your six rows are testing strategies that cannot vary, and the remaining rows are three put values.
- **Only three of eleven grid rows are represented,** and the winner / best-active-hedge labels are never compared.

So the honest reading is not "the spec was sufficient" but "the spec was sufficient **for the six quantities I checked**." Those are different claims, and the second one is the defensible one.

This is not a request to manufacture a discrepancy. It is the habit that matters: when a test comes back clean, the next question is *"what would this test have failed to catch?"* — and then you widen it until it either finds something or you can say precisely what it rules out. A full eleven-row comparison across all five strategies is 55 cells and would have taken you one more paste of the LLM's grid.

**Two smaller items**

1. **Your recommendation memo has directions but no magnitudes.** §C reads: *"the premium reduces the net proceeds compared to the forward hedge"* and *"the put option also participates in this appreciation after accounting for the premium."* Both true, neither actionable. The two numbers a CFO needs are sitting in your own workbook: the put floors you **$417,500** below the forward (14,607,500 − 14,190,000), and it does not overtake the forward until `S_T` reaches `(14,607,500 + 212,500) / 12,500,000 = **1.18560**`, which is **2.9% above spot**. State those and §C stops being a description of shapes and becomes a decision.

2. **The premium is not carried to settlement.** `15,122,625 − 212,500` nets a today-dollar against a settlement-dollar. At your own `R_USD`, `212,500 × 1.0404 = $221,085`, so the like-for-like floor is **$14,181,415** and the breakeven moves up about 7 bp. Small here, and it does not change the recommendation — but you were rigorous about tenor-matching the *rates*, and this is the same principle applied to a cash flow.

Also worth tidying: §3 Step 3 writes the formula as `USD at Maturity = USD Today × (1 + R_USD)`, dropping the `× T_DAYS ÷ 360` factor that Step 1 states explicitly. The substitution is fine; the formula as written is not, and an LLM reading only that line would build the wrong thing — which is precisely the failure mode this stage exists to surface.

**Repo polish — 3.2 points, plus a path bug**

`LICENSE` and the one-line repository description are the two scored gaps.

Separately, and please fix this one: your recommendation memo is committed at

```
docs/decisions/docs/decisions/2026-08-15-scheuermann-tech-services-hedge-recommendation.md
```

The directory is doubled. It is reachable, so it did not cost you points, but every relative link to it will be wrong, it will not be found where a reader looks for it, and it reads as an artifact of a drag-and-drop upload rather than a deliberate commit. Move it to `docs/decisions/` and delete the nested folder.

— Treasury

---

### How to work this review — professional workflow

Treat this PR the way an analyst treats feedback from Treasury — a review is a proposal to engage with, not a checklist to rubber-stamp:

1. **Read it yourself first.** Understand each point and form your own view before changing anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM (pushback pass).** Paste this review and your spec into your AI assistant and ask it to (a) explain anything you're unsure of more deeply, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change. You're building judgment, not just executing edits.
3. **Decide, then draft the changes with the LLM.** For the points you accept, have the AI help implement them — you specify exactly what and why. Your spec is the prompt; precise in, correct out.
4. **Verify — non-negotiable.** Re-run your own checks (`scripts/recalc.py`, the parity tie-out, sensitivity continuity, no error cells) and confirm the numbers before you commit. An AI will hand you a confident wrong edit; verification is what makes the result *yours*.
5. **Close the loop on the PR.** Reply in the thread with what you changed, what you pushed back on and why, then commit and push. Writing down the reasoning is exactly how this works on a real team.

*This is the same human-in-the-loop discipline the whole project is built on: the LLM drafts, you edit and verify, and you own the result.*
