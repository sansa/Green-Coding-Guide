---
title: "Core Principles"
nav_order: 3
permalink: /core-principles/
---

# 3. Core Principles of Green Coding

The following principles provide a technology-agnostic foundation for reducing the runtime energy footprint of software systems. They apply across domains and abstraction levels — from individual functions to distributed architectures. Domain-specific practices that implement these principles are described in [Section 4](/domain-practices/).

## 3.1 Avoid Unnecessary Work

The most effective way to reduce energy consumption is to avoid computation, data processing, and communication that provide no functional value. This includes logic that executes on every request but only matters in rare cases, data that is fetched but never used, and operations triggered by events that do not require them.

Unnecessary work is often invisible because it is spread across many small operations, each of which appears harmless in isolation. The cumulative effect — across many users, requests, or devices — can be substantial. Identifying it requires profiling rather than inspection: the most expensive code is rarely the most obvious.

This principle also applies at the architectural level. Processing that could be avoided through better system design — such as repeatedly re-deriving the same result from a database — represents unnecessary work regardless of how efficiently that work is performed.

## 3.2 Avoid Repeated Work

Repeated execution of the same work increases cumulative energy footprint without adding functional value. Reuse, caching, and memoisation can significantly reduce redundant resource usage when applied deliberately and measured carefully.

The key qualifier is *deliberately and measured*. Caching is not universally beneficial: it consumes memory, introduces staleness risk, and only saves energy when the hit rate is high enough to justify the overhead. The same applies to precomputation — it shifts work from request time to build or startup time, which may reduce per-request energy but increases system-level energy if the precomputed results are rarely used.

Repeated work is a particularly common problem in distributed systems, where the same data is fetched independently by multiple components, and in frontend applications, where components re-render in response to state changes that do not affect them.

## 3.3 Move and Store Less Data

Data movement and storage contribute significantly to energy consumption, particularly in distributed systems and mobile environments. Transmitting data over a network, reading and writing to disk, and copying data between memory regions all consume energy in proportion to the volume transferred.

Reducing payload sizes, eliminating redundant serialisation and deserialisation, compressing data where the CPU cost of compression is outweighed by the energy savings from reduced transfer, and avoiding unnecessary persistence are all practical expressions of this principle.

The principle applies at every scale: reducing the number of columns selected in a database query, trimming the fields returned by an API, filtering log output, and removing unused dependencies from a software bundle are all instances of moving and storing less data.

## 3.4 Prefer Efficient Abstractions

Abstractions improve maintainability but can hide expensive operations. A convenient method call, a high-level framework feature, or a widely-used library may conceal significant computational overhead — particularly when used in a hot path, at scale, or in a resource-constrained environment.

Green coding encourages awareness of the resource and energy implications of the abstractions in use. This does not mean abandoning abstractions in favour of low-level code — in most cases, the maintainability cost of doing so far outweighs the energy benefit. It means being deliberate: understanding what an abstraction does under the hood, choosing lighter alternatives when they exist and are functionally equivalent, and profiling rather than assuming that a familiar tool is efficient.

This principle is particularly relevant when selecting frameworks, runtimes, serialisation formats, and communication protocols, where the choice affects the entire system rather than a single operation.

## 3.5 Make Energy Footprint Visible

Without measurement, energy efficiency remains speculative. Developers cannot reliably identify the most energy-intensive parts of a system through inspection alone, and energy improvements cannot be verified without a baseline to compare against.

Visibility through profiling and comparison is essential for evidence-based green coding. This means establishing energy or resource baselines before optimising, measuring the impact of changes rather than assuming them, and making energy-related information available to the team — through dashboards, CI metrics, or code review artefacts — so that it can inform decisions over time.

Visibility also means acknowledging uncertainty. Energy measurement tools have limitations (see [Section 7](/tradeoffs/)), and results should be interpreted comparatively and transparently rather than treated as precise absolute values.
