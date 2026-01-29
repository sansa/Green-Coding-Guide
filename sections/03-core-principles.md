---
title: "Core Principles"
order: 3
permalink: /core-principles/
---

# 3. Core Principles of Green Coding

The following principles provide a technology-agnostic foundation for reducing the runtime energy footprint of software systems.

## 3.1 Avoid Unnecessary Work

The most effective way to reduce energy consumption is to avoid computation, data processing, and communication that provide no functional value.

## 3.2 Avoid Repeated Work

Repeated execution of the same work increases cumulative energy footprint. Reuse, caching, and memoization can significantly reduce redundant resource usage when applied deliberately.

## 3.3 Move and Store Less Data

Data movement and storage contribute significantly to energy consumption, particularly in distributed systems. Reducing payload sizes, serialization overhead, and redundant persistence lowers energy footprint.

## 3.4 Prefer Efficient Abstractions

Abstractions improve maintainability but can hide expensive operations. Green coding encourages awareness of the resource and energy implications of abstractions.

## 3.5 Make Energy Footprint Visible

Without measurement, energy efficiency remains speculative. Visibility through profiling and comparison is essential for evidence-based green coding.