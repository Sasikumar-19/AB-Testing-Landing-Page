# A/B Testing — Full Experimentation Framework (v2)

**295,065 sessions · 2 groups · 3 methodologies · 1 recommendation**

This repo contains two versions of the same landing page experiment.
v1 ran a basic Z-test and stopped. v2 builds a complete experimentation
framework — power analysis before the test, frequentist and Bayesian
inference on the result, and a multi-armed bandit simulation to show
when classic A/B testing isn't the right tool at all.

---

## Why v2 Exists

v1 had one real gap: it answered *"is this significant?"* but not
*"was this test designed correctly?"* or *"how confident are we,
exactly?"* or *"what should we do differently next time?"*

A one-line p-value is not an experimentation framework. v2 is.

---

## Dataset

| Property | Value |
|---|---|
| Total sessions (after cleaning) | 295,065 |
| Control group (old page) | 147,202 users · 17,561 conversions |
| Treatment group (new page) | 147,863 users · 17,724 conversions |
| Control conversion rate | 11.93% |
| Treatment conversion rate | 11.99% |
| Observed difference | +0.057pp |
| Misassigned users removed | Yes — users in wrong group/page combinations dropped |
| Duplicate user IDs removed | Yes |

---

## Results — All Three Methods

### 1. Power Analysis (should happen BEFORE the test)

Answers: *"How many sessions do we actually need?"*

| Target Lift | Required n/group | Required total | Status |
|---|---|---|---|
| +1.0pp (ambitious) | 17,081 | 34,162 | ✅ Well covered |
| +0.5pp (realistic) | 67,157 | 134,314 | ✅ Well covered |
| +0.2pp (minimal) | 415,304 | 830,608 | ❌ Underpowered for this |

**Key insight:** With 295,065 sessions, this experiment had 80%+ power
to detect a lift of +0.5pp or larger. It could NOT reliably detect a
+0.2pp lift — that would need 830K sessions. The null result is not
because the test was underpowered for any meaningful lift. The new page
genuinely does not improve conversion at a level worth caring about.

---

### 2. Frequentist Z-Test (two-sided)

| Metric | Value |
|---|---|
| Control conversion | 11.93% (17,561 / 147,202) |
| Treatment conversion | 11.99% (17,724 / 147,863) |
| Absolute difference | +0.057pp |
| 95% CI on difference | [−0.0018, +0.0029] |
| Z-statistic | 0.4763 |
| P-value (two-sided) | 0.6338 |
| Cohen's h (effect size) | 0.0018 — negligible |
| Decision | Fail to reject H₀ |

**Interpretation:** p = 0.634 means there is a 63% chance of seeing
a difference this large (or larger) purely by random chance, even if the
two pages are identical. The 95% confidence interval on the difference
spans zero comfortably — [−0.18pp, +0.29pp]. Cohen's h of 0.0018 is
effectively zero effect size by any standard benchmark.

---

### 3. Bayesian A/B Test

Answers: *"What is the actual probability the new page is better?"*

| Metric | Value |
|---|---|
| Control posterior mean | 0.1193 |
| Control 95% credible interval | [0.1177, 0.1210] |
| Treatment posterior mean | 0.1199 |
| Treatment 95% credible interval | [0.1182, 0.1215] |
| **P(Treatment > Control)** | **68.2%** |
| P(Control > Treatment) | 31.8% |
| Expected relative lift | +0.479% |
| 95% CI on relative lift | [−1.480%, +2.473%] |

**Interpretation:** There is only a 68.2% probability the new page is
genuinely better — well below the 95% threshold needed to act with
confidence. The credible intervals on both groups overlap almost
completely. The expected relative lift of +0.48% is so small it would
be undetectable operationally. This richer answer is more useful than
the binary Z-test result — it tells you *how uncertain* you should be,
not just that you can't reject the null.

---

### 4. Multi-Armed Bandit Simulation (10,000 rounds)

Answers: *"What happens if we use smarter traffic allocation?"*

| Strategy | Conversions | Traffic to Best Arm |
|---|---|---|
| Classic A/B (50/50) | 1,184 | 50.0% (fixed) |
| Thompson Sampling | 1,164 | 24.2% |
| Epsilon-Greedy (ε=0.1) | 1,148 | 5.7% |

**Interpretation:** Classic A/B actually wins here — and that's the
correct result. When the two arms are nearly identical (0.057pp apart),
bandit algorithms waste exploration budget trying to figure out which
arm is better when neither meaningfully is. The bandit insight is not
that it always beats A/B — it's that it outperforms when there is a
real difference to exploit. Here there isn't, so the fixed 50/50 split
loses nothing. Thompson Sampling's 24% traffic to the treatment arm
reflects that it correctly identified the treatment as *slightly*
better — but not confidently enough to commit.

---

## What All Three Methods Agree On

| Approach | Answer | Threshold |
|---|---|---|
| Frequentist | p = 0.634 | Needs p < 0.05 |
| Bayesian | P(B>A) = 68.2% | Needs > 95% |
| Bandit | Classic A/B wins by 20 conversions | No clear exploitation target |
| **Verdict** | **Do not deploy the new page** | |

---

## Final Business Recommendation

**Do not deploy the new page.**

All three methods reach the same conclusion through different lenses.
The observed +0.057pp lift is not statistically significant, not
practically meaningful, and not large enough to exploit via adaptive
traffic allocation.

More importantly — this result is not a failed experiment. It is a
successful one. The experiment correctly prevented deploying a page that
doesn't work, at a cost of zero revenue loss.

**Next steps for the product team:**
1. Run qualitative research (user interviews, session recordings) to
   understand what users actually want from the page
2. Test copy and CTA changes rather than visual redesigns — evidence
   in CRO literature consistently shows these drive larger conversion lifts
3. If a future test is designed, target a minimum detectable effect of
   **+0.5pp** and collect **at least 134,314 sessions per group** based
   on this power analysis
4. Consider Thompson Sampling for future tests where you expect a real
   difference — it will find and exploit it faster than 50/50 A/B

---

## Version Comparison

| | v1 (Internship) | v2 (This notebook) |
|---|---|---|
| Hypothesis test | One-tailed Z-test | Two-tailed Z-test + Cohen's h + 95% CI |
| Pre-test design | ❌ None | ✅ Power analysis across 3 MDE scenarios |
| Uncertainty quantification | Binary p-value | ✅ Full posterior distribution + credible intervals |
| Bayesian inference | ❌ | ✅ Beta posteriors + P(B>A) via Monte Carlo |
| Adaptive algorithms | ❌ | ✅ Epsilon-greedy + Thompson Sampling simulation |
| Business recommendation | "Not significant" | ✅ Full recommendation + next test design guidance |
| Charts | Static Seaborn | ✅ Interactive Plotly (power curve, posteriors, bandit) |

---

## Notebook Structure

| Section | What it covers |
|---|---|
| 1 — Setup | Imports: NumPy, Pandas, SciPy, Statsmodels, Plotly |
| 2 — Load & clean | Remove misassigned users, drop duplicates, confirm group sizes |
| 3 — Power analysis | Required n across 3 MDE scenarios · interactive power curve chart |
| 4 — Z-test | Two-sided proportions Z-test · Cohen's h · 95% CI on difference |
| 5 — Bayesian A/B | Beta posteriors · P(B>A) via 200K Monte Carlo samples · credible intervals · lift distribution |
| 6 — Bandit simulation | Epsilon-greedy · Thompson Sampling · Classic A/B over 10K rounds |
| 7 — Comparison | Three-method summary table |
| 8 — Recommendation | Full business recommendation + next test design guidance |

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.11+ | Core language |
| NumPy | Monte Carlo sampling, simulation |
| pandas | Data loading and cleaning |
| SciPy | Beta distribution (posteriors) |
| Statsmodels | Z-test, power analysis, effect size |
| Plotly | Interactive charts |
| Jupyter | Analysis environment |

---
