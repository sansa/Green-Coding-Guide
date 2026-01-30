---
title: "Checklists"
order: 8
permalink: /checklists/
---

# 8. Green Coding Checklists

These checklists support reflection and discussion rather than compliance.

## 8.1 Frontend – Web
- Are renders and requests minimized?
- Has bundle size increased?
- Is data fetched only when needed?

## 8.2 Frontend – Mobile
- Are background tasks minimized?
- Are sensors and wakeups justified?
- Is offline behavior supported?

## 8.3 Backend – Services
- Is work per request minimized?
- Are queries selective and paginated?
- Are caches bounded?

## 8.4 Data Pipelines
- Is recomputation avoided?
- Is data retention controlled?
- Is processing frequency justified?

## 8.5 AI Training
- Is model size justified?
- Are experiments reused?
- Is hardware utilization high?

## 8.6 AI Inference
- Is the smallest effective model used?
- Are prompts minimized?
- Is caching applied where safe?