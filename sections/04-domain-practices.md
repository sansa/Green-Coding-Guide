---
title: "Green Coding Practices by Domain"
order: 4
permalink: /domain-practices/
---

# 4. Green Coding Practices by Domain

Green coding principles apply across domains, but their practical impact depends on execution context. This section organizes practices by domain, focusing on **dominant contributors to runtime energy footprint**.

## 4.1 Frontend – Web Applications

Energy footprint drivers include JavaScript execution, rendering, and network activity.

Key practices:
- Minimize unnecessary re-renders and layout thrashing
- Reduce bundle size via splitting and lazy loading
- Avoid chatty API interactions
- Use caching and HTTP mechanisms effectively

Because improvements apply across many user devices, frontend efficiency can have large cumulative impact.

## 4.2 Frontend – Mobile Applications

Mobile energy footprint is dominated by battery usage, network radios, and sensors.

Key practices:
- Reduce background activity and wakeups
- Batch network requests and support offline workflows
- Optimize rendering and scrolling
- Use sensors conservatively and at appropriate precision

## 4.3 Backend – Services and APIs

Backend energy footprint is driven by CPU usage per request, data access, and network communication.

Key practices:
- Reduce work per request
- Avoid N+1 queries and over-fetching
- Use bounded, measured caching
- Minimize payload size and serialization overhead

## 4.4 Backend – Data Processing and Storage

Data pipelines consume energy through prolonged execution and large-scale I/O.

Key practices:
- Avoid full recomputation
- Use incremental and checkpointed processing
- Apply data retention and lifecycle policies
- Align processing frequency with actual needs

## 4.5 AI Development – Training

AI training energy footprint is dominated by accelerator runtime.

Key practices:
- Right-size models and datasets
- Avoid redundant experiments
- Apply early stopping and efficient training techniques
- Maximize hardware utilization

## 4.6 AI Usage – Inference

Inference energy footprint scales with request volume.

Key practices:
- Use the smallest effective model
- Minimize prompt and input size
- Cache deterministic outputs and embeddings
- Batch requests where feasible