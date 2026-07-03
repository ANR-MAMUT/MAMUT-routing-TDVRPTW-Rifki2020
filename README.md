# Rifki2020 TDVRPTW benchmark family (real Lyon road-network travel times)

Satellite benchmark repository of [MAMUT-routing](https://github.com/ANR-MAMUT/MAMUT-routing) (ANR MAMUT, ANR-22-CE22-0016), mounted there as `benchmarks/TDVRPTW/Rifki2020`. Self-contained: instances, canonical arrival-time-function (ATF) sidecars and best-known solutions ship together.

Canonical Duration-Minimization TDVRPTW instances built from the real-world time-dependent travel times of Rifki, Chiabaut & Solnon (2020): shortest travel times computed on the Lyon road network from a realistic traffic simulation, at ideal spatial granularity (σ = 100) and 12-minute temporal granularity (K = 60 steps of 720 s over a 12 h horizon; see the granularity section below). 6 sizes (10–60 customers) × 30 instances = 180 instances, named `Rifki-<id>` under `n=<customers>`.

## Format

Each instance ships three artifacts per size bucket `n=<customers>`:

- `Rifki-<id>.vrp.json` — instance data in the MAMUT shape: depot index 0 (no duplicated end depot), mandatory display `coordinates`, `horizon` `[0, 43200]`, and a `td` block referencing the ATF sidecar with its storage-independent sha256.
- `Rifki-<id>.atf.json.gz` — the canonical ground truth: one arrival-time NDCPWLF per arc of the complete graph (all sidecars are gzipped in this satellite; the sha256 covers the uncompressed canonical bytes).
- `Rifki-<id>.bks.Duration.json` — best known solution under the `Duration` objective; costs are always the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`).

## FIFO restoration (the defining preprocessing of this family)

The raw data gives piecewise *constant* travel times per arc (integer seconds per 720 s step), which violate the FIFO property at every boundary where travel time decreases. The canonical ATFs apply the **arrival-time lower envelope** — the greatest non-decreasing minorant of the raw arrival function, `α̂(t) = min_{t' ≥ t} (t' + τ(κ(t')))`, i.e. the exact form of the classic MD92 transformation (Malandraki & Daskin 1992): decreasing steps get a slope −1 descent placed entirely before the boundary; upward jumps are kept and encoded as **vertical steps** of the NDCPWLF (duplicate `xs`; evaluation at a step returns the smallest value, which is the envelope's own value there). The transformation is parameter-free, exact on the integer raw data, and changes values only on descent tails where FIFO restoration is mathematically unavoidable.

## Why K = 60 (and not the finest K = 120)

The original distribution ships **five temporal granularities** of the same underlying Lyon simulation (step lengths l ∈ {720, 60, 24, 12, 6} minutes, i.e. K ∈ {1, 12, 30, 60, 120} steps). This family pins **K = 60** (l = 12 min), for deliberate reasons:

- **Breakpoint weight.** After FIFO restoration, K = 120 yields ≈ 238 breakpoints per arc versus ≈ 119 at K = 60 — and sidecar sizes halve accordingly. At K = 60 the family sits in the same per-arc weight class as the other canonical TD families (Ari2018/Vu2020 ≈ 150 breakpoints per arc; Dabia2013 ≈ 10) instead of being a 2× outlier; composition-based solver operations scale linearly in breakpoint counts.
- **K = 60 is not a lossy compression of K = 120.** The granularities are independent published aggregations of the same simulation, not derivable from one another (at 96% of step boundaries the l = 12 value matches neither the first, the min, nor the mean of the two corresponding l = 6 values; envelope travel times differ by ~12% on average between the two). Choosing K = 60 selects one published discretization of the real data, every bit as authentic as the finest one.
- **Nothing was invalidated.** The granularity was fixed while the canonical family had no best-known solutions, so no published result depends on the earlier K = 120 draft.

## Attribute provenance — deliberately not the original [RCS20] time windows

The original benchmark distributes separate time-window files (`timewindow_<n>_<k>_<w>.txt`, horizon split into 2/4/6 disjoint slots). This family deliberately does **not** use them: they are degenerate for benchmarking purposes (many windows clustered near the horizon start, many identical), original service times are uniformly 180 s, and no demands or fleet data exist (the original problem is not capacitated). Instead, the time windows, gaussian service times, demands, vehicle capacities and fleet sizes are the values generated once by Onyr's legacy TDVRPTW-benchmarks pipeline (2024), pinned here verbatim; every instance's `metadata.legacy_attribute_source` names the exact legacy file. The original TW files remain available upstream for anyone studying the [RCS20] setting.

Display coordinates are not part of the raw data (real road network): they are a deterministic stress-layout embedding (SMACOF, classical-MDS init) of the time-averaged travel-time matrix; per-instance embedding quality (Kruskal stress-1 ≈ 0.05–0.12, Pearson r ≈ 0.99) is recorded in `metadata.coordinates_method`. Coordinates are informative-only: the checker never reads them.

Population pipeline: `populate_td_rifki2020` v2 (curation tooling maintained outside this repository; links will be published with the benchmark paper); every artifact's `generator` block records the pipeline name and version.

## Provenance and licensing

Underlying travel-time data: Rifki, Chiabaut & Solnon (2020), Lyon road network with a traffic simulation built from real-world data (see the TDGPDP page of Christine Solnon). MAMUT-authored curation artifacts are distributed under the MIT license; this notice does not relicense the underlying third-party data.
