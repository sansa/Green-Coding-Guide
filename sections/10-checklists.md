---
title: "Checklists"
nav_order: 10
permalink: /checklists/
---

# 10. Green Coding Checklists

These checklists support reflection and discussion during code review, design review, or retrospectives, not as compliance audits. Use them as prompts to identify energy-related risks, not as a pass/fail gate.

## 10.1 Frontend – Web
- Are re-renders limited to the components that actually changed?
- Has bundle size been measured recently, and is it justified?
- Is data fetched only when needed, and not fetched more than once for the same content?
- Are images and media served at appropriate resolution and in modern formats (WebP, AVIF)?
- Are third-party scripts audited for necessity and load impact?
- Are CSS transitions or the Web Animations API preferred over JavaScript animation loops?
- Are polling or interval-based patterns replaced with event-driven or push alternatives where feasible?
- Is HTTP caching configured correctly for static and semi-static assets?

## 10.2 Frontend – Mobile
- Are background tasks minimized, consolidated, and deferred to system-scheduled windows?
- Are sensors and wakeup receivers unregistered when not actively needed?
- Is offline behavior supported to avoid redundant network round trips?
- Are network requests batched so the radio can power down between active periods?
- Is rendering limited to visible content using view recycling or lazy list patterns?
- Is location accuracy matched to actual requirements (coarse vs. fine)?
- Are push notifications used in preference to polling for state updates?
- Are animations and transitions implemented using hardware-accelerated APIs?

## 10.3 Backend – Services
- Is work per request profiled, and are hot paths free of avoidable operations?
- Are database queries selective, indexed, and paginated rather than fetching full tables?
- Are N+1 query patterns eliminated through eager loading or batched queries?
- Is payload size minimized, and are only required fields returned?
- Are caches bounded, and are hit rates measured to confirm they are earning their memory cost?
- Are synchronous operations that could be safely deferred moved off the critical path?
- Are idle connections, thread pools, and executors bounded to avoid resource exhaustion under low load?
- Is serialization format appropriate to the use case (e.g., binary formats for internal high-throughput paths)?

## 10.4 Data Pipelines
- Is recomputation of unchanged partitions avoided through checksums, timestamps, or incremental frameworks?
- Are checkpoints used so that failed runs can resume rather than restart from scratch?
- Is processing frequency matched to actual downstream consumption rate?
- Is cold or infrequently accessed data tiered to lower-energy storage?
- Is data retention governed by a defined lifecycle policy that prevents unbounded growth?
- Are pipeline stages profiled to identify which step dominates energy and execution time?
- Is parallelism matched to available hardware, avoiding both under- and over-provisioning?

## 10.5 AI Training
- Is model capacity justified by the task, or would a smaller architecture meet requirements?
- Is experiment tracking in place to prevent duplicate or near-duplicate training runs?
- Is GPU/TPU utilization consistently high, or are there data-loading or preprocessing bottlenecks?
- Is early stopping applied when validation metrics plateau?
- Is mixed-precision training (FP16/BF16) used where hardware and framework support it?
- Are datasets filtered or deduplicated to remove samples that do not contribute to model quality?
- Are hyperparameter searches bounded and stopped when diminishing returns are evident?

## 10.6 AI Inference
- Is the smallest model that meets quality requirements used in production?
- Are inputs and prompts as concise as possible, removing redundant context?
- Is caching applied to deterministic or repeated queries to avoid redundant inference?
- Are requests batched where latency requirements allow?
- Has model quantization been evaluated as a way to reduce per-request compute?
- Is inference endpoint utilization monitored, and is capacity adjusted to avoid sustained idle over-provisioning?
- Are embedding computations cached rather than recomputed on each request?
