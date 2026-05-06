---
title: "Measurement and Feedback"
nav_order: 5
permalink: /measurement-feedback/
---

# 5. Measurement and Feedback
Measurement is the foundation of evidence-based green coding. Without it, energy efficiency remains speculative: developers cannot reliably identify the most energy-intensive parts of a system through inspection alone, optimisation efforts cannot be verified, and regressions accumulate undetected. This section describes what software energy measurement involves, which approaches and tools are available, how experiments should be designed and interpreted, and what the current limitations of measurement practice are.

The role of measurement in green coding parallels its role in performance engineering. Just as performance optimisation requires profiling before tuning, energy optimisation requires measurement before and after any change. The discipline is the same: establish a baseline, make a targeted change, measure again, and compare. Without a documented baseline, there is no way to determine whether a change improved, degraded, or had no effect on energy consumption.

---

## 5.1 What to Measure

Software energy measurement can target several distinct quantities, and the choice of what to measure determines what conclusions can be drawn.

### Energy per Unit of Work

Energy per unit of work is the most directly actionable metric for comparing implementations. Expressing consumption as joules per request, joules per transaction, or joules per inference normalises for execution time and allows fair comparison between implementations that differ in throughput or latency.

A change that reduces energy per request by 20% is a genuine improvement regardless of whether it makes the system faster or slower in wall-clock terms. Energy is the integral of power over time — a process that draws 60 W for 5 seconds and one that draws 30 W for 12 seconds have similar energy footprints (300 J vs 360 J) despite very different power profiles, and the distinction matters for choosing between them.

### Power Draw Over Time

Power draw over time, measured in watts, reflects the instantaneous rate of energy consumption and is useful for identifying sustained high-load states, understanding the shape of a workload, and detecting the moment at which a specific operation drives consumption.

Power readings are the raw output of most measurement tools; they must be integrated over the execution period to yield energy, and power alone is an insufficient basis for comparing two implementations.

### Resource Utilisation Metrics

Resource utilisation metrics — CPU utilisation, memory allocation rate, network bytes transferred, and storage I/O volume — do not directly measure energy but are strongly correlated with it.

They are widely available through standard monitoring infrastructure without additional instrumentation and serve as practical proxies for continuous monitoring and anomaly detection. CPU utilisation in particular is a reliable leading indicator of processor energy consumption on homogeneous hardware and is the basis for many estimation approaches used in cloud environments where direct energy measurement is unavailable.

### Carbon-Adjusted Consumption

Carbon-adjusted consumption extends energy measurement to climate impact by multiplying energy consumed by the carbon intensity of the electricity grid at the time and location of execution.

This is most relevant at the infrastructure and deployment strategy level — where choices about cloud region, energy procurement, and workload scheduling interact with grid carbon intensity — rather than at the level of individual code changes. For most software-level optimisation work, energy is the more directly actionable target.

---

## 5.2 Measurement Approaches

Accurate software energy measurement requires choosing the right approach for the hardware environment and the granularity of information needed. Two broad categories are available:

- hardware-based external measurement
- software-based internal measurement

They differ in what they cover, what they require to deploy, and what conclusions they reliably support.

### Hardware-Based External Measurement

Hardware-based external measurement uses a physical power meter placed between the power supply and the system under test. This approach captures the total energy consumed by the entire device — processor, memory, storage, networking, and cooling overhead — without requiring any access to or modification of the software running on the system.

It is applicable to a wide range of devices, from embedded single-board computers to servers, and to any type of software, including those running in runtimes that do not expose software-level energy counters.

The main limitation is that hardware meters measure total system power, not the contribution of individual processes, components, or code paths. Isolating the energy attributable to a specific piece of software therefore requires running that software alone on the test system — ideally in a containerised environment — so that the difference between idle and active power can be attributed to the software under test.

Within the VISIIRI project, the University of Turku has evaluated a broad range of hardware meters and developed open-source tooling to support this measurement approach. Their recommendations and configurations are documented in the PowerGoblin project (University of Turku, 2024).

### Software-Based Internal Measurement

Software-based measurement accesses energy data through operating system interfaces or hardware performance counters, without requiring physical instrumentation.

The most widely available interface on commodity x86 hardware is Intel's Running Average Power Limit (RAPL), which exposes per-package and per-domain energy counters — covering the CPU, DRAM, and on some processors the uncore and integrated graphics — at millisecond-level resolution.

RAPL readings can be accessed through:
- the Linux `perf` subsystem
- the MSR interface
- user-space libraries and tools including:
  - `perf stat`
  - `turbostat`
  - `PAPI`

NVIDIA GPUs expose power consumption through the NVML interface.

On Linux, tools such as:
- `collectd`
- `nmon`
- `atop`

can aggregate CPU, memory, network, and storage utilisation data alongside power readings, providing a richer picture of the resource drivers behind energy consumption.

### Limitations of Software-Based Measurement

The key limitation of software-based measurement is scope:
- RAPL covers the processor and DRAM subsystems but excludes storage, networking, peripherals, and cooling.
- Software meters that derive energy estimates from utilisation data rather than hardware counters introduce additional model-dependent uncertainty.
- In virtualised and cloud environments, hardware energy counters are typically unavailable to guest operating systems.

### Combining Hardware and Software Measurement

In practice, hardware and software measurement are complementary.

- Hardware measurement provides whole-system ground truth for isolated workloads.
- Software measurement provides process-level or component-level attribution and is deployable where physical instrumentation is impractical.

Used together — hardware readings establishing the absolute reference and software counters providing the breakdown by process or resource type — they support the most informative analyses.

### Measurement in Cloud and Virtualised Environments

Cloud and shared infrastructure present a distinct challenge. In virtualised environments, the physical host's energy counters are mediated by the hypervisor and not exposed to tenants.

The practical alternative is utilisation-based estimation:
- CPU utilisation
- memory utilisation
- infrastructure monitoring metrics

These can be converted into energy estimates using empirical power models for specific instance types.

Frameworks such as:
- Cloud Carbon Footprint
- the Green Software Foundation's Software Carbon Intensity (SCI) specification

formalise this approach.

The accuracy of such estimates varies with the quality of the underlying hardware characterisation, but they can identify high-consumption components and support trend analysis over time. Results should be presented with explicit uncertainty ranges and not treated as precise measurements.

---

## 5.3 Experimental Design and Baseline Establishment

The validity of any energy measurement depends on the conditions under which it is collected. Measurement without controlled conditions produces numbers that cannot be meaningfully compared.

### Representative Workloads

Measurement cases should be derived from real usage scenarios: representative patterns of user or system behaviour that reflect the actual workload the software is expected to handle.

A measurement case is operationalised as a script that automates the interactions to be measured — executing the same sequence of requests, operations, or inputs every time — so that each repetition is as identical as possible.

The energy footprint of a full system can then be estimated by combining per-scenario measurements with estimates of how frequently each scenario occurs in production.

### Establishing a Baseline

Establishing a baseline is the first and most critical step.

A baseline measurement characterises the energy footprint of the current implementation under the defined workload, providing the comparison point against which any change is evaluated.

A useful baseline is:
- representative of real usage patterns
- repeatable across runs
- fully documented

Documentation should include:
- hardware configuration
- operating system
- measurement tool
- workload parameters
- thermal conditions
- background processes
- number of repetitions

### Warm-Up and Steady-State Conditions

Warm-up and steady-state conditions must be accounted for in environments with:
- just-in-time compilation
- adaptive CPU frequency scaling
- cache effects

JVM-based and .NET runtimes, for example, produce substantially different energy profiles during startup compared to steady state.

Measurements should begin after a warm-up period sufficient to reach a stable state, and the warm-up approach should remain consistent across baseline and candidate runs.

### Repetition and Statistical Treatment

Energy measurements are not deterministic.

Variability is introduced by:
- background system activity
- operating system scheduling
- thermal throttling
- cache state

A single measurement is therefore insufficient.

Results should report:
- mean values
- standard deviation
- number of repetitions

An observed improvement whose magnitude is smaller than the standard deviation should not be treated as a confirmed finding.

### Isolation and Noise Reduction

Isolation reduces noise.

Recommended practices include:
- containerised execution
- minimising background processes
- disabling dynamic frequency scaling
- using fixed-performance CPU governors where possible

Although full isolation may not be achievable in production environments, it is strongly recommended for controlled experiments.

### Differential Measurement

Differential measurement compares:
- a baseline implementation
- a candidate implementation

under identical conditions.

The same workload, hardware, system state, and environmental conditions should be used for both runs. Systematic biases that affect both measurements equally tend to cancel out, making relative comparisons more reliable than isolated absolute values.

---

## 5.4 Interpreting and Communicating Results

Raw measurement outputs require careful interpretation before they can reliably inform engineering decisions.

### Distinguishing Power from Energy

Conflating power and energy is one of the most common mistakes.

A change that reduces peak power draw but increases execution time may increase total energy consumption. The correct comparison metric is energy consumed per unit of work, not peak or average power alone.

### Avoiding Incorrect Causal Attribution

An observed reduction in energy consumption should not automatically be attributed to a specific code change.

Other factors may influence results:
- warmer caches
- reduced background load
- thermal conditions
- scheduler behaviour

Controlled differential measurement mitigates this risk.

### Reporting Uncertainty

Results should always include:
- number of repetitions
- variability measures
- context for interpreting significance

An improvement of 8% with a standard deviation of 6% is not a robust result.

### Effective Communication

Energy data becomes more useful when translated into interpretable forms.

Effective approaches include:
- percentage change relative to baseline
- trend visualisation across versions
- workload-scaled projections
- production traffic equivalences

Visual representations such as:
- trend lines
- comparison charts
- energy profiles

significantly improve comprehension and willingness to act on measurement results (Joof et al., 2025).

For non-technical stakeholders, energy data is often most persuasive when connected to:
- operational cost reduction
- infrastructure efficiency
- estimated carbon impact

---

## 5.5 Limitations of Current Measurement Practice

Honest measurement practice requires acknowledging what current methods cannot reliably deliver.

### Scope Gaps

No single measurement tool covers all energy-consuming components of a modern system.

Examples:
- RAPL excludes storage and networking
- hardware meters lack process-level attribution
- monitoring systems expose utilisation but not energy directly

Conclusions should therefore always be qualified in terms of measurement scope.

### Cloud and Virtualisation Constraints

In shared cloud environments:
- host energy counters are inaccessible
- workloads are affected by multi-tenancy
- hardware heterogeneity introduces uncertainty

Utilisation-based estimation is often the only feasible option, but results should be treated as estimates rather than exact measurements.

### Workload Representativeness

Controlled benchmarks may not reflect real production usage patterns.

An optimisation that improves benchmark energy consumption may have little real-world impact if the optimised path is rarely executed in production.

Where possible, measurement scenarios should be derived from:
- production logs
- telemetry
- observed workload distributions

### Thermal and Environmental Variability

Environmental conditions introduce unavoidable variability:
- ambient temperature
- thermal throttling
- operating system activity
- cooling behaviour

This is why repeated runs and statistical analysis are necessary rather than optional.

### Interpreting Limitations Correctly

These limitations do not invalidate software energy measurement.

Relative improvements measured:
- under controlled conditions
- against a documented baseline
- across multiple repetitions

are sufficiently reliable to support engineering decisions.

The goal is not perfect precision, but sufficient confidence to make better decisions about software energy footprint.

Measurement uncertainty is also discussed further in Section 7.5.

---

# References

Green Software Foundation (2023) *Software Carbon Intensity (SCI) Specification*. Version 1.0. Available at: https://sci.greensoftware.foundation

Joof, M.B. (2025) *Green Coding in Practice: A Software Framework for API Energy Efficiency Measurement and Feedback*. Master's thesis. Lappeenranta–Lahti University of Technology LUT.

Joof, M.B., Khan, M.A., Oyedeji, S. and Porras, J. (2025) *Integrating API Energy Profiling into Developer Workflows: The VerdeFlow Prototype*. In Proceedings of the IEEE/ACM International Conference on Software Engineering: Workshop on Green and Sustainable Software (ICSE-GREENS). ACM.

Treiber, M. (2015) *RAPL in action: Experiences in using RAPL for power measurements*. ACM Transactions on Modeling and Performance Evaluation of Computing Systems, 1(1), pp.1–26.

University of Turku, Software Engineering Group (2024) *PowerGoblin User Manual*. Available at: https://visiiri.tt.utu.fi/powergoblin/manual (Licensed under CC BY-NC 4.0).