# Changelog — Rifki2020 TDVRPTW BKS

All notable changes to the curated `Rifki2020` TDVRPTW best-known solutions (BKS) are recorded here. Objective: **Duration** (duration minimization — the depot departure time of each route is a decision variable). Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order — so any strict improvement is real. Reminder: time windows and vehicle attributes are Onyr's curated ones, not the original [RCS20] TW files (see README.md), so these BKS are not comparable with results on the original setting.

## 2026-07-07

13 BKS improved by exact solves — **proven optimal**: kayros 0.3.0 lera branch-price-and-cut (HiGHS backend, warm-started from the previous BKS, TL 600 s), from the certification campaign over all families n≤50. Three n=10 entries were far from optimal (Rifki-26: 9045 → 4632, −48.8%; Rifki-25: −32.8%; Rifki-6: −26.5%); the other ten range −0.1% to −2.5% across n=20..50. Certificates: optimal under checker-exact route costs and standard LP/pricing tolerances, completeness modulo Lera epsilon dominance. The same campaign certified 91 of the other stored TDVRPTW BKS of this family optimal as stored (27 of 30 at n=10; the other 3 are among today's improvements).

Structured optimality metadata: all 104 BKS of this family proven optimal by that campaign now carry a machine-readable `metadata.optimality` object — prover, certificate wording, proven optimum, dual bound, wall time (schema: `OptimalityMetadata`, mamut-routing-lib ≥ 0.4.0). Additionally, **Rifki-1 (n=40) replaced**: the campaign proved the optimum 11961.999999999982 on a route set different from the stored one (cost 11961.999999999985, 3.6e-12 above) — a dust-level difference, but the checker-verified cost is authoritative, so the proven route set is now stored.

## 2026-07-06

177 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

## 2026-07-05

179 BKS added, reaching full 180/180 coverage, from the initial large-scale seeding sweep across all four TD families run on 2026-07-04 (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

## 2026-07-03

Family populated at the canonical K = 60 temporal granularity (180 instances + envelope-FIFO ATF sidecars) with 1 BKS — `Rifki29` (n=10), the single legacy solution still valid at K = 60, re-priced by the canonical checker.
