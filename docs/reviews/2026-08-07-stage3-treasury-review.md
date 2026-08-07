# Stage 3 review — tech services build & audit · Treasury sign-off

Ella — the workbook is well built and the audit note is the best-*structured* one in the cohort: every finding has a stated test, a result, and notes. That template is exactly right. My push is on what you pointed it at. Five findings, five PASSes, "no material issues identified" — and the workbook does contain something worth flagging that the audit walked past.

| Criterion | Score |
|---|---|
| Contract compliance | 47.3 / 50 |
| Structure & presentation | 25 / 25 |
| Audit note | 25 / 25 |
| **Total** | **98 / 100** |

**What you did well — and why it matters**

- **Your CIRP check is a real control, not decoration.** `D11` computes the implied forward from `S0_in` and both rate legs, `D13` differences it against `F0_in`, and `D15` returns PASS/REVIEW against a stated tolerance. Building the test *into the workbook* — so it re-evaluates every time an input changes — is far stronger than checking once by hand and declaring it fine.
- **You wrote down that your tolerances are assumptions.** The note at `A24` — that the tolerance bands reflect expected rounding under CIRP and "are not intended to mask a modeling error" — is a sentence a good auditor writes and a careless one omits. You anticipated the obvious challenge and answered it in place.
- **Finding 3 tests behaviour, not appearance.** Changing `S0_in` and confirming the sensitivity table recalculated is a *dynamic* test. Most students eyeball the grid and call it verified; you perturbed an input and watched the model respond, which is the only way to prove the wiring is live.
- **The audit template itself is reusable.** Test Performed / Result / Notes is a structure you can carry into Stage 5 and into professional work unchanged. Several strong students in this cohort lost points purely because their findings weren't legible as findings; yours never will be.

**The core issue — an audit that finds nothing usually means the tests were too easy**

Every one of your five findings returns PASS, and four of them test whether something *exists*: do the named ranges exist, are formulas used, does the table recalculate, do the checks run. Those are presence checks. They confirm the workbook was assembled as specified — they cannot detect a workbook that was assembled correctly and is still wrong.

Two concrete places to point a harder test:

- **Your amount tolerance is loose enough to hide a real break.** `D21` sets the Forward-vs-Money-Market agreement band at **$15,000** on a €12.5M notional. When covered interest parity actually holds and both legs are built from the same inputs, that difference should land in the low hundreds of dollars — for comparison, another student's came in at $231 on a larger notional. A $15,000 band will return PASS across a range of genuine modelling errors. Size a tolerance to the precision you *expect*, not to a number comfortably wider than anything you might see; otherwise the green light is unearned.
- **Finding 5 spotted the hardcode and then excused it.** You noted the Option Hedge uses a manually entered `S_T` (`Option Hedge!B5 = 1.1522`) and concluded it "is appropriate." It's defensible as an illustrative single scenario — but you had the thread in your hand and let go. The follow-up question is: if `S_T` is typed here and *driven* on the Sensitivity tab, can those two ever disagree? That's the kind of question that turns an observation into a finding.

**One build note:** your sensitivity grid's row index and step fractions (`A6:B16`) are typed literals, which is what puts your formula ratio at 76%. It works, but a hand-typed grid silently goes out of sync the moment someone widens the range. Drive it from a single step-size input and generate the rows — that's the 2 points, and more importantly it's the habit.

**Next — Stage 4**

Already in and reviewed separately. Carry one thing forward: at Stage 4 your inputs finally become mutually consistent, which means your CIRP check becomes genuinely informative for the first time. Tighten that $15,000 band before you trust what it tells you.

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
