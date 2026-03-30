---
title: "Software Energy Footprint"
nav_order: 8
permalink: /footprint/
---

# 8. Software Energy Footprint

Understanding the software energy footprint requires moving beyond intuition about which code is "fast" or "efficient" and toward a systematic understanding of how software behaviour translates into energy consumption. This section provides the conceptual foundation for that understanding.

## 8.1 What Is a Software Energy Footprint?

The **software energy footprint** is the total amount of energy consumed by a hardware system as a direct consequence of executing a given piece of software over a defined period or workload. It is not a property of code at rest; it emerges during execution and depends on what the software does, how often, on what hardware, and under what conditions.

This runtime character distinguishes the software energy footprint from lifecycle perspectives that also consider the energy embedded in hardware manufacturing, data centre construction, or end-of-life disposal. Those concerns are real and significant, but they are largely outside the control of a software developer making implementation decisions. The runtime footprint, by contrast, is directly shaped by software design choices and is therefore the primary focus of green coding practice.

The footprint is also inherently **workload-dependent**. The same codebase can have a substantially different energy footprint depending on the size of the input, the number of concurrent users, the frequency of requests, and the presence or absence of data in a cache. This means that energy cannot be meaningfully characterised by inspecting code alone; it requires measurement under representative conditions.

## 8.2 How Software Consumes Energy

Software does not consume energy directly. It causes hardware components to perform work, and those components consume energy. Understanding which components are involved, and what drives their consumption, is essential for identifying where optimisation effort will have the greatest effect.

**Processor (CPU/GPU/TPU)**
The processor is typically the dominant energy consumer during active computation. Dynamic power, which is the energy consumed by switching transistors, scales with clock frequency and the number of active circuits. Modern processors use frequency and voltage scaling to reduce power when demand is low, but high utilisation drives them toward their peak power envelope. Software that keeps processors in high-utilisation states for extended periods contributes disproportionately to energy consumption.

**Memory**
Dynamic RAM consumes energy during both access and idle operation. Memory reads and writes, particularly those that miss the CPU cache and reach main memory, are significantly more energy-intensive per operation than operations that remain in the L1 or L2 cache. Software with poor data locality, large working sets, or high allocation and garbage collection rates drives elevated DRAM energy consumption.

**Network interfaces**
Transmitting and receiving data over a network interface requires energy proportional to the volume of data transferred, plus fixed overhead for connection establishment and protocol negotiation. In mobile environments, radio interfaces (LTE, 5G, Wi-Fi) have particularly high energy costs relative to the data transferred, and they remain in an elevated power state for a period after a transmission ends, known as the *radio tail*, which makes many small transmissions significantly more expensive than fewer larger ones (Pathak et al., 2012).

**Storage**
Solid-state storage consumes energy during read and write operations, with writes typically more costly than reads. Spinning disk storage has additional mechanical energy costs. Software that performs unnecessary I/O, such as writing redundant logs, re-reading unchanged files, or performing frequent small random writes, contributes to storage energy consumption in proportion to access frequency and volume.

**Display**
In user-facing applications, the display can be a significant energy consumer, particularly on mobile devices, where screen brightness and rendering complexity directly affect battery drain. Software that keeps the screen active unnecessarily, renders at full resolution when a lower resolution would suffice, or triggers frequent screen updates for non-visible changes increases display energy consumption.

## 8.3 Attributing Energy to Software

A central challenge in green coding is that energy is measured at the system level as total power drawn from the supply, while responsibility for that consumption is distributed across the operating system, runtime, frameworks, application code, and concurrent processes. Attributing a specific fraction of system energy to a specific software component is technically difficult and subject to significant uncertainty.

Several approaches are used in practice:

**Hardware performance counters**
Modern processors expose energy measurement registers that software can query. Intel's Running Average Power Limit (RAPL) interface, for example, provides per-package and per-domain (CPU, DRAM, uncore) energy readings at high resolution. These are the most direct software-accessible energy measurements available on commodity hardware, but they cover only the processor and memory subsystems and are not available in all virtualised or cloud environments.

**System-level power measurement**
External power meters measure the total energy consumed by a system at the wall or chassis level. This captures all components but does not attribute consumption to individual processes or software components. It is most useful for comparing two versions of a workload running in isolation under identical conditions.

**Model-based estimation**
When direct measurement is unavailable, as is commonly the case in cloud and shared infrastructure, energy consumption can be estimated from CPU utilisation, memory usage, and workload characteristics using empirical models. The accuracy of such estimates varies considerably and depends on the quality of the underlying hardware characterisation. The Cloud Carbon Footprint project and the Software Carbon Intensity (SCI) specification from the Green Software Foundation are examples of frameworks that formalise this approach.

Regardless of the measurement approach, attribution is most reliable when a single workload runs in isolation on known hardware. In production environments, attribution is always partial, and results should be interpreted accordingly (see also [Section 7, Measurement Uncertainty](/tradeoffs/)). A practical demonstration of API-level attribution combines containerised execution, which isolates the target workload from background processes, with a software energy profiler such as PowerJoular to attribute power consumption to specific endpoints and commit versions. This approach achieves strong agreement with direct profiler readings (Pearson r = 0.94) while requiring only commodity hardware and open-source tooling (Joof et al., 2025; Joof, 2025).

## 8.4 Energy, Power, and Carbon: Clarifying the Terms

These three related concepts are often conflated, but they are distinct and each has different implications for measurement and optimisation.

**Power** (measured in watts, W) is the rate of energy consumption at an instant in time. A process that draws 50 W is consuming energy at that rate. Power alone does not tell you the total energy consumed, as that depends on how long the power is drawn.

**Energy** (measured in joules, J, or kilowatt-hours, kWh) is the product of power and time: *E = P × t*. A process that draws 50 W for 10 seconds consumes 500 J. Energy is the right unit for comparing the total cost of executing a workload, because it accounts for both the power draw and the duration. A faster but more power-hungry implementation is not necessarily more energy-efficient than a slower, lower-power one, because the total energy (the integral of power over time) determines the footprint.

**Carbon emissions** (measured in grams or kilograms of CO₂-equivalent, CO₂e) represent the climate impact of energy consumption. The same kilowatt-hour of electricity produces different carbon emissions depending on the source: near-zero for solar or wind, and substantially more for coal or gas. Carbon intensity, measured as grams of CO₂e per kilowatt-hour, varies by grid region, time of day, and season. Optimising for energy footprint and optimising for carbon footprint generally point in the same direction, but the relationship is not fixed, and geographic or temporal shifting of workloads can reduce carbon emissions independently of energy reduction.

For most software optimisation work, **energy** is the most directly actionable metric. Carbon accounting is most relevant at the infrastructure and deployment strategy level, where choices about cloud region, energy procurement, and workload scheduling interact with grid carbon intensity.

## 8.5 Factors That Shape the Software Energy Footprint

The software energy footprint is not determined by code alone. It is the product of an interaction between software behaviour and execution environment. The principal factors are:

**Algorithmic complexity**
The asymptotic complexity of an algorithm determines how its resource consumption scales with input size. An O(n²) algorithm applied to a large dataset may consume orders of magnitude more energy than an O(n log n) alternative. Algorithmic choices therefore have the highest leverage on energy footprint at scale, and they are made early in development, making them among the most important decisions to evaluate from an energy perspective (Pereira et al., 2017). This effect is measurable even at small scale: replacing a custom nested-loop sort with a language built-in sort in a backend API reduced power draw by 23% and CPU utilisation from 34.5% to 26.1% under a standardised request workload (Joof et al., 2025; Joof, 2025).

**Data access patterns**
Whether computations operate on data that is in CPU cache, in main memory, or on disk has a large effect on energy consumption. Cache-friendly data structures and access patterns, particularly those that exhibit spatial and temporal locality, are significantly more energy-efficient than those that scatter memory accesses across large address ranges.

**Concurrency and parallelism**
Parallelism increases hardware utilisation and can reduce wall-clock time, but it does not necessarily reduce energy consumption. If multiple threads perform redundant work, or if synchronisation overhead is high, parallel execution may consume more energy than sequential execution of the same task. Conversely, well-structured parallelism can improve energy efficiency by completing work faster and allowing hardware to return to lower-power states sooner.

**Hardware and runtime characteristics**
The same code produces different energy footprints on different hardware. Processor microarchitecture, memory hierarchy design, and the energy proportionality of the hardware platform (how closely idle power scales with load) all affect the measured footprint. Runtime characteristics such as JIT compilation, garbage collection, and interpreter overhead add additional variability.

**Deployment and scaling**
The number of instances running, the load they serve, and the idle time between requests all affect aggregate energy consumption. A highly optimised single instance running at very low utilisation may consume more energy than a moderately optimised instance running at high utilisation, due to the fixed energy cost of keeping a server running regardless of load.

---

## References

Green Software Foundation (2023) *Software Carbon Intensity (SCI) Specification*. Version 1.0. Available at: https://sci.greensoftware.foundation

Pathak, A., Hu, Y.C. and Zhang, M. (2012) *Where is the energy spent inside my app? Fine-grained energy accounting on smartphones with Eprof*. Proceedings of the 7th ACM European Conference on Computer Systems (EuroSys), pp.29–42.

Pereira, R., Couto, M., Ribeiro, F., Rua, R., Cunha, J., Fernandes, J.P. and Saraiva, J. (2017) *Energy efficiency across programming languages: How do energy, time, and memory relate?* Proceedings of the 10th ACM SIGPLAN International Conference on Software Language Engineering, pp.256–267.

Treiber, M. (2015) *RAPL in action: Experiences in using RAPL for power measurements*. ACM Transactions on Modeling and Performance Evaluation of Computing Systems, 1(1), pp.1–26.

Joof, M.B. (2025) *Green Coding in Practice: A Software Framework for API Energy Efficiency Measurement and Feedback*. Master's thesis. Lappeenranta–Lahti University of Technology LUT.

Joof, M.B., Khan, M.A., Oyedeji, S. and Porras, J. (2025) *Integrating API Energy Profiling into Developer Workflows: The VerdeFlow Prototype*. In Proceedings of the IEEE/ACM International Conference on Software Engineering: Workshop on Green and Sustainable Software (ICSE-GREENS). ACM.
