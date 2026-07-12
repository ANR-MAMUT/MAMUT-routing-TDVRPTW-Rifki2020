# Changelog — Rifki2020 TDVRPTW BKS

All notable changes to the curated `Rifki2020` TDVRPTW best-known solutions (BKS) are recorded here. Objective: **Duration** (duration minimization — the depot departure time of each route is a decision variable). Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order — so any strict improvement is real. Reminder: time windows and vehicle attributes are Onyr's curated ones, not the original [RCS20] TW files (see README.md), so these BKS are not comparable with results on the original setting.

## 2026-07-12

**9 further BKS stamped proven optimal** by the weekend top-up of the exact re-certification campaign (2026-07-11/12, Grid'5000, about 3000 additional runs over the instances the 2026-07-10/11 campaign left open; time limit 1800 s per run). Protocol unchanged from the 2026-07-10/11 campaign: four independent exact solves (cold and warm starts crossed with the two labeling modes) agreeing on the value, an audited exact-pricing phase in every run, zero checker-infeasible priced columns, and canonical-checker re-validation at stamping time. New certificates: Rifki-13, Rifki-18, Rifki-24 (n=20); Rifki-14, Rifki-19 (n=30); Rifki-9, Rifki-27, Rifki-30 (n=40); Rifki-20 (n=50). This family now carries 71 certified TDVRPTW BKS.

## 2026-07-11

**62 BKS stamped proven optimal** (`metadata.optimality`): optimality certificates RETURN to this family after the 2026-07-08 retraction, under a strictly stronger protocol and a repaired prover (the pricing-ladder termination defect that produced the retracted certificates is fixed; the four-run protocol was validated to catch the failure class). Each stamp certifies four independent exact solves (cold and warm starts x two labeling modes) agreeing on the value, an audited exact-pricing phase in every run, zero checker-infeasible priced columns, and canonical-checker re-validation at stamping time. Instances whose runs disagreed, timed out, or priced a checker-infeasible column carry NO stamp. All 30 instances at n=10 are certified; larger sizes are certified only where all four runs closed and agreed.

## 2026-07-10 (second entry)

3 further BKS improved under the same four-run agreement protocol as the Vu2020 fold of the same date (exact solver, cold/warm x two labeling modes, checker re-validated, no optimality stamps): Rifki-26 n=10, Rifki-17 n=20, Rifki-17 n=50.

## 2026-07-10

3 BKS improved by exact solves recorded as ordinary best-known solutions (no optimality stamps): Rifki-30 n=40 (15743 -> 15691, -0.33%), Rifki-20 n=50 (18888 -> 18810, -0.41%), Rifki-21 n=50 (16957 -> 16951, -0.04%), from a two-arm re-certification attempt (kayros lera branch-price-and-cut, HiGHS backend, cold and warm runs in separate processes, TL 600 s, Grid'5000). The campaign's certificates were withheld: certification on this family's stepwise travel-time functions remains blocked because certified values were again warm-start-dependent on some instances, and additionally proved sensitive to the build toolchain (the same instance and input certify different "optima" under two compilers). Each folded solution was re-priced by the canonical checker before writing (checker cost authoritative); a checker-valid strict improvement stands regardless of certificate validity.

## 2026-07-08

**Optimality certificates retracted (all 104 stamped BKS of this family) and 29 further BKS improved.** The head-to-head campaign below produced checker-valid solutions strictly below 29 of the 2026-07-07 "proven optima" (margins 1 to 136 duration units, up to 1.46%; e.g. Rifki-16 n=20: 8485 → 8361) — those certificates are refuted by counterexample. The failure was reproduced deterministically: the prover's certified value depends on its warm start (three runs on Rifki-16 n=20 certified 8485, 8376 and 8361 as "optimal", each accepted as feasible by its own model), i.e. its pricing step is incomplete on this family's stepwise travel-time functions — the near-vertical segments used to continuize the step discontinuities for the solver's piecewise-linear machinery amplify its internal epsilon tolerances into route-level cost errors, so improving routes can be silently discarded and the dual bound is not sound. The canonical checker (exact arithmetic, no epsilons) remains authoritative and confirms every counterexample. All `metadata.optimality` stamps of this family are therefore retracted, including the 75 whose values were never contradicted (the campaign matched 66 of those values exactly without improving them, which supports the values but restores them to ordinary best-known status). The 29 counterexample solutions are now folded as regular BKS (mean -0.37%, largest -1.46%) through the standard improve-only, checker-authoritative pipeline. Re-certification will follow once the exact solver handles stepwise travel times exactly.

61 of 180 BKS improved (mean -0.62%, largest single improvement -1.78%) by a 20,808-run anytime-strategy head-to-head campaign on Grid'5000: kayros 0.4.0.dev0 (TD-ILS, TD-ACO+LS, and an ACO-then-ILS budget split, all over the granular time-dependent local search), per-size time limits (120 s for n<=30, 300 s for n<=60, 600 s for n<=100), seeds {42, 123, 456}, single-threaded runs. Improve-only fold: for each instance the campaign-best solution was re-priced by the canonical checker before writing (checker cost authoritative); stored BKS marked proven optimal were left untouched.

## 2026-07-07

13 BKS improved by exact solves — **proven optimal**: kayros 0.3.0 lera branch-price-and-cut (HiGHS backend, warm-started from the previous BKS, TL 600 s), from the certification campaign over all families n≤50. Three n=10 entries were far from optimal (Rifki-26: 9045 → 4632, −48.8%; Rifki-25: −32.8%; Rifki-6: −26.5%); the other ten range −0.1% to −2.5% across n=20..50. Certificates: optimal under checker-exact route costs and standard LP/pricing tolerances, completeness modulo Lera epsilon dominance. The same campaign certified 91 of the other stored TDVRPTW BKS of this family optimal as stored (27 of 30 at n=10; the other 3 are among today's improvements).

Structured optimality metadata: all 104 BKS of this family proven optimal by that campaign now carry a machine-readable `metadata.optimality` object — prover, certificate wording, proven optimum, dual bound, wall time (schema: `OptimalityMetadata`, mamut-routing-lib ≥ 0.4.0). Additionally, **Rifki-1 (n=40) replaced**: the campaign proved the optimum 11961.999999999982 on a route set different from the stored one (cost 11961.999999999985, 3.6e-12 above) — a dust-level difference, but the checker-verified cost is authoritative, so the proven route set is now stored.

## 2026-07-06

177 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

## 2026-07-05

179 BKS added, reaching full 180/180 coverage, from the initial large-scale seeding sweep across all four TD families run on 2026-07-04 (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

## 2026-07-03

Family populated at the canonical K = 60 temporal granularity (180 instances + envelope-FIFO ATF sidecars) with 1 BKS — `Rifki29` (n=10), the single legacy solution still valid at K = 60, re-priced by the canonical checker.
