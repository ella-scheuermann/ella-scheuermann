# Stage 2 review — tech services · Treasury sign-off

Ella — this spec does something most Stage 2 submissions don't: it designs for the stage that hasn't happened yet. Declaring, input by input, *where the live number will come from at Stage 4* turns your inputs table into a data-sourcing plan rather than a list of placeholders. That is the difference between a spec that describes a workbook and one that governs how the workbook will be maintained.

| Criterion | Score |
|---|---|
| Named-range contract & tab architecture | 30 / 30 |
| Calculation flow | 30 / 30 |
| Validation & sensitivity plan | 20 / 20 |
| Reproducibility & prompt log | 20 / 20 |
| **Total** | **100 / 100** |

**What you did well — and why it matters**

- **The "Stage 4 Data Source" column is the strongest idea in this spec.** Committing in advance that `S0_in` comes from a spot quote, `R_USD` from the 1-Year Treasury yield, and `F0_in` from a forward quote means Stage 4 becomes execution rather than invention. It also makes you accountable: when a source turns out to be unavailable, you have to say so explicitly instead of quietly substituting.
- **You labelled every indicative value as indicative.** Marking each placeholder "indicative – replaced with live market data at Stage 4" prevents the single most damaging Stage 2/3 error, which is a reader treating a made-up number as a real one. Numbers escape their context constantly; yours carry their status with them.
- **You designed for input substitution without structural change.** Stating up front that live data must drop into the named-range cells *without altering formulas or layout* is the actual engineering requirement behind the named-range contract, and you wrote it as a requirement rather than leaving it implicit.
- **The tab architecture assigns each sheet a job.** Cover, Legend/Key, Inputs, one tab per hedge family, Sensitivity, Notes — each with a stated purpose. Separating inputs from calculations from outputs is what makes a model auditable by someone who didn't build it.

**To push it further (real-desk nuance)**

- **Watch your tenor on `R_FC`.** You list the European Central Bank as the source for the euro rate. The ECB's headline rates are *overnight policy* rates, and your model has a 360-day horizon — an overnight rate and a one-year rate are different instruments, and mixing tenors is the most common way a carry calculation goes quietly wrong. Your `R_USD` choice (1-Year Treasury) already has the right tenor; the euro leg should match it. *(You caught this yourself at Stage 4 and moved to a 1-year German government yield — good instinct, and worth understanding as a general rule, not a one-off fix.)*
- **`R_FC` is missing its unit.** The table reads `2.576` where every other rate reads `4.059%`. On a real desk, a rate without a unit is how someone eventually divides by 100 twice. Cheap to fix, expensive to miss.
- **Say what the parity check should produce, numerically.** You specify that the money-market tab includes a covered-interest-parity check. Go one step further and pre-commit the tolerance: how close must `F_implied` sit to `F0_in` before you'd call it a pass? Deciding that *before* you see the answer is what keeps a check honest — otherwise the tolerance gets sized to whatever the model happens to produce.

**Next — Stage 3**

Build straight from §3 and §4, then audit against your own §7 validation rules. The bar for the audit note is at least three *substantive* findings — and the word doing the work there is substantive. An audit where every check returns PASS is a weaker document than one that finds a real defect, because it usually means the tests weren't hard enough. Go looking for the thing that's wrong.

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
