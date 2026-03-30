---
title: "Trade-offs and Limitations"
nav_order: 7
permalink: /tradeoffs/
---

# 7. Trade-offs, Limitations, and Design Tensions

Green coding involves balancing energy footprint against other software quality attributes. The tensions described here are real, and they cannot be resolved by rule of thumb alone. Each requires empirical evaluation in context.

## 7.1 CPU vs. Memory

Reducing CPU work often increases memory usage — for example, precomputing and caching results avoids repeated calculation at the cost of heap space. The net energy impact depends on the access pattern: memory is relatively cheap to *hold*, but expensive to *allocate, copy, and garbage-collect* at high rates.

**Decision heuristic:** Prefer the memory-intensive option when data is accessed repeatedly within a bounded time window and the cache hit rate is demonstrably high. Prefer the compute-intensive option when access is infrequent, data volume makes caching uneconomical, or GC pressure from cached objects outweighs computation savings. Measure both approaches under a representative workload before deciding.

## 7.2 Network vs. Computation

Reducing data transfer can require additional computation — for example, compressing a payload before sending it or pre-aggregating data server-side instead of transferring raw records. Conversely, skipping computation to send data faster keeps the network active longer and may increase device-side processing costs.

**Decision heuristic:** When user-perceived latency is not the binding constraint, prefer to reduce transfer volume. When latency is critical, benchmark whether the energy cost of additional processing (compression, aggregation) is recovered by reduced transmission time. Note that network energy costs vary substantially between wired, Wi-Fi, and mobile radio contexts. This trade-off has been observed empirically in API-level profiling: adding GZIP compression to API responses reduced network payload but increased CPU load relative to an in-memory caching variant, resulting in higher power consumption (250 mW vs 240 mW) despite smaller transfer sizes — illustrating that compression is not unconditionally beneficial (Joof et al., 2025; Joof, 2025).

## 7.3 Efficiency vs. Maintainability

Highly optimised code is often harder to understand, test, and change. An energy optimisation that couples components tightly, removes useful abstractions, or relies on undocumented hardware behaviour may reduce energy footprint now while increasing it later through defects and rework.

**Decision heuristic:** Apply energy-motivated optimisations that do not materially increase code complexity first. When a more invasive optimisation is necessary, document the performance characteristic being preserved, isolate the optimised code behind a well-defined interface, and add benchmarks that will surface regressions if the optimisation is later removed or bypassed.

## 7.4 Local vs. System-Wide Effects

Optimising a single component in isolation can shift energy consumption elsewhere in the system rather than eliminating it. A faster client-side operation may generate more server requests. A more efficient database query may increase application-layer processing. A reduced network payload may require more CPU to reconstruct on the receiving end.

**Decision heuristic:** Before optimising a component, trace the data flow through the system to identify downstream effects. Prioritise optimisations that reduce total system energy, not just local metrics. When system-wide measurement is impractical, at minimum verify that the upstream and downstream components are not adversely loaded by the change.

## 7.5 Measurement Uncertainty

Energy measurement has inherent limitations. Software-accessible counters (such as Intel RAPL) measure processor and DRAM subsystems but exclude storage, network, and peripheral devices. Cloud and virtualised environments often do not expose hardware energy counters at all. Results vary with CPU frequency scaling, thermal throttling, background load, and measurement granularity.

**Decision heuristic:** Report relative improvements between a baseline and a candidate under identical conditions, rather than absolute energy values. Repeat measurements across multiple runs and, where possible, across different hardware configurations before drawing conclusions. State measurement conditions explicitly — the tool used, the system state, the workload, and the number of repetitions. Treat a result as preliminary until it has been validated in a representative deployment environment.

## 7.6 Diminishing Returns

Beyond eliminating major inefficiencies — redundant computation, unnecessary data transfer, idle resource holding — additional optimisation yields progressively smaller energy savings at increasing implementation cost. The first 20% of optimisation effort often accounts for 80% of the achievable improvement.

**Decision heuristic:** Prioritise interventions by estimated energy impact per unit of implementation effort. Use profiling to identify the dominant contributors to energy footprint before investing in optimisation. Stop optimising a component when further improvements require disproportionate complexity or risk, and redirect effort toward higher-impact areas identified by measurement.

---

## References

Joof, M.B. (2025) *Green Coding in Practice: A Software Framework for API Energy Efficiency Measurement and Feedback*. Master's thesis. Lappeenranta–Lahti University of Technology LUT.

Joof, M.B., Khan, M.A., Oyedeji, S. and Porras, J. (2025) *Integrating API Energy Profiling into Developer Workflows: The VerdeFlow Prototype*. In Proceedings of the IEEE/ACM International Conference on Software Engineering: Workshop on Green and Sustainable Software (ICSE-GREENS). ACM.
