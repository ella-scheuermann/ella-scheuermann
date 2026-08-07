# Stage 4 review — tech services market data & population · Treasury sign-off

Ella — the strongest thing in this memo is something you may not have registered as a decision: you overrode your own Stage 2 spec. You had written "European Central Bank" as the source for `R_FC`, and when you got here you used a one-year German government yield instead, because the ECB's rate is overnight and your model runs 360 days. Noticing a tenor mismatch in your own plan and correcting it is exactly the judgment this stage is meant to develop.

| Criterion | Score |
|---|---|
| Data quality & provenance | 50 / 50 *(instructor-adjusted — see below)* |
| Model resolves cleanly | 30.4 / 33 |
| Lab cross-check | 17 / 17 |
| **Total** | **98 / 100** |

**A note on the grade.** The automated scanner checks provenance by matching a fixed list of vendor names (Bloomberg, Yahoo, ECB, FRED) and scored your memo 1 of 3 on source signals, which would have put you at 88. That is the scanner being narrow, not your memo being thin: you cite three named, dated external sources — Investing.com, the U.S. Department of the Treasury, and Trading Economics — which is precisely what the rubric asks for. I verified it by hand and scored full provenance.

**What you did well — and why it matters**

- **Every input carries a source, a value, a unit, and a retrieval time.** Including the ones you *didn't* source. Marking `FC_AMT`, `PREM_PUT`, `PREM_CALL`, and `T_DAYS` as scenario-assigned with "N/A" retrieval is not padding — it tells an auditor those numbers were never market-derived, so nobody later mistakes a course parameter for a live quote.
- **You justified the rate choices on tenor.** One-year Treasury for the dollar leg, one-year German government yield for the euro leg, both matched to a 360-day horizon. Tenor matching is the most common silent error in a carry model, and you got both legs right and said why.
- **You derived the forward instead of inventing it.** Showing the CIP formula, the substituted inputs, and the result (1.1686) means a reader can reproduce your forward from your own inputs. And the comparison against the Stage 2 indicative rate of 1.16924 — a difference of 0.00064 — is a genuine validation: your live-data build agrees with your planned build to within six ten-thousandths.
- **Your assumptions section states what's excluded.** No transaction costs, taxes, brokerage, or bid-ask spread. Naming the boundary of a model is what stops a reader treating its output as an executable price.

**To push it further (real-desk nuance)**

- **Now tighten the tolerance.** This is the payoff from Stage 3. Your inputs are finally mutually consistent, so your Forward-vs-Money-Market difference should now be small and *stable*. Recompute it and reset `Money Market Hedge!D21` from $15,000 to something near the precision you actually observe. A check calibrated to loose inputs stops being a check once the inputs are good.
- **A CIP-implied forward is not a market forward.** You flagged that no live one-year quote was used, which is the right disclosure. Be precise about what your near-zero parity residual proves: it proves your forward and money-market legs share inputs. It cannot validate against the market, because no market forward price entered the calculation. The real gap — cross-currency basis plus dealer spread — is invisible to your model by construction.
- **Quantify your data sensitivity.** You've documented where every number came from; the next step is what happens if one is wrong. At a one-year tenor, a 25bp error in `R_FC` moves the CIP forward by roughly 0.0028 — about $35,000 on EUR 12.5M. That single sentence tells a CFO how much to care about your source quality.

**Next — Stage 5**

Hand the workbook and your Stage 2 spec to an LLM, get its analysis, then break it. Recompute at least three outputs by hand with explicit arithmetic — the forward proceeds, the put floor, and the crossover spot where the option overtakes the forward. Then write the recommendation in a CFO's voice, framed on risk tolerance rather than on which strategy happens to top your grid: the forward buys certainty, the put buys a floor plus upside for a premium, and which one is right depends on how much variance the business can absorb. Your Stage 5 retrospective should also say plainly which parts of your Stage 2 spec didn't survive contact with live data — the ECB tenor call is already one of them.

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
