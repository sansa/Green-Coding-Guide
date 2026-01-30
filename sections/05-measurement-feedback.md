---
title: "Measurement and Feedback"
order: 5
permalink: /measurement-feedback/
---

# 5. Measurement and Feedback

Green coding requires empirical validation. Energy footprint depends on workload, execution context, and scale, making intuition unreliable.

## 5.1 Why Measurement Is Necessary

Energy consumption arises from interactions between resources. Performance alone is not a reliable proxy for energy footprint.

## 5.2 What to Measure

Relevant indicators include:
- CPU usage
- Memory usage
- Network I/O
- Storage I/O
- Energy or power (measured or estimated)

## 5.3 Measurement Granularity

Energy footprint can be observed at:
- system level
- service or component level
- API or operation level
- code level

Different stakeholders benefit from different levels of granularity.

## 5.4 Comparative Measurement

Comparing versions, implementations, or configurations is often more informative than absolute values. Consistency matters more than precision.

## 5.5 Tool-Agnostic Principles

Green coding favors reproducible, low-friction, workflow-aligned measurement approaches rather than specific tools.

## 5.6 Feedback and Interpretation

Visualization and contextual interpretation turn raw measurements into actionable insight.