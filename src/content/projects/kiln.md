---
title: 'Kiln'
description: 'An open platform for building, evaluating, packaging, deploying, governing, and operating production agents and intelligent applications.'
publishDate: 'Aug 01 2026'
lane: ai-agents
tags:
  - planned
seo:
  description: 'Kiln — an open platform covering the full lifecycle of production agents: build, evaluate, package, deploy, govern, operate.'
---

**Note:** This project is **planned**: the repository is staked out and the scope is defined, but implementation has not started. The write-up below describes intent, not shipped behavior.

**Project Overview:**
Getting an agent to work in a notebook is not the hard part. The hard part is everything after: proving it works on more than the three cases you tried, packaging it so it runs the same way somewhere else, deploying it without hand-rolling infrastructure, knowing what it did after the fact, and keeping it inside the bounds someone signed off on.

Today those steps live in different tools that don't know about each other—an eval harness here, a container registry there, tracing in a third place, and policy as a document nobody reads. Kiln is an attempt to treat the agent lifecycle as one platform concern rather than six disconnected ones.

## The Lifecycle

1. **Build** — author agents with a consistent structure rather than a bespoke layout per project.
2. **Evaluate** — measure behavior against defined criteria so changes are regressions or improvements, not vibes.
3. **Package** — produce a reproducible artifact that runs identically across environments.
4. **Deploy** — ship packaged agents to a runtime without rebuilding the delivery path each time.
5. **Govern** — enforce policy, permissions, and approval boundaries as part of the platform rather than as convention.
6. **Operate** — observe running agents, understand failures, and intervene while they're live.

## Objectives

1. **One path, not six tools** — make the route from prototype to production a single well-worn track instead of an integration project.
2. **Evaluation as a gate, not a report** — behavior checks that block bad changes rather than describing them afterward.
3. **Open by default** — no lock-in to a single model provider, orchestration framework, or hosting target.
4. **Production-shaped from the start** — governance and operability treated as first-class, not retrofitted once something breaks.

## Status

_Planning. Repository initialized; architecture and platform boundaries are being worked out._
