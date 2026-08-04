---
title: 'Seeker'
description: 'A native iOS podcast app built on two ideas most players get wrong: your listening contexts should not collide, and search should understand what you mean. Multiple independent queues plus semantic discovery.'
publishDate: 'Apr 14 2026'
lane: apps
tags:
  - in-progress
seo:
  description: 'Seeker — a native iOS podcast app with multiple independent queues and semantic, intent-based episode discovery.'
---

**Note:** This project is **in progress**: the iOS app is under active development against a defined PRD, targeting a January 2027 release.

**Project Overview:**
Seeker is a podcast app built around two ideas most podcast apps get wrong: your listening contexts shouldn't collide, and search should understand what you mean, not just what you typed.

> **North star:** Seeker exists so that a person's listening life can hold more than one interest at a time—without any of them getting lost.

## The Problem

Podcast apps funnel everything into a single master queue. Play something new and whatever you were "up next" on either gets bumped to the bottom or silently drops out while lingering in the UI as if it's still there. Worse, the queue makes no distinction between contexts: an episode about true crime sits next to an episode about distributed systems, and you're stuck skipping around to find something that fits your current mood.

Discovery is stuck in the past too. Search is keyword matching against titles and show names. There's no way to ask for what you actually want—"true crime shows that focus on the detectives' side of the investigation"—and get episodes that match the idea rather than the words.

## The Two Pillars

### 1. Multiple, Independent Queues

Instead of one master queue, you create as many as you want ("True Crime", "Tech"). Each queue is created and managed by you, and each has its own playback state and progress. Playing an episode in one queue never affects, reorders, or drops anything in another. Unrelated interests stay separated without you losing your place in either.

### 2. Semantic Discovery Search

Search understands intent. A query like "true crime shows that focus on the retelling from the police and detectives on the case" surfaces episodes matching that angle even when none of those exact words appear in the title or description—powered by embeddings over show and episode metadata.

## Product Invariants

Four promises are treated as non-negotiable and enforced by automated tests on every change:

1. **Isolation** — actions in one queue never affect another's contents, order, or position.
2. **No silent mutation** — episodes only change through direct user action or an explicit, user-enabled setting.
3. **Durable position** — queue state and resume timestamps survive app termination, device restarts, and arbitrary time gaps.
4. **Truthful UI, reversible AI** — the display matches storage, and every AI action requires confirmation with one-tap dismissal.

## MVP Scope

**Core differentiators**

- Multiple manually-created queues with fully independent playback state.
- Semantic search over show and episode metadata for natural-language discovery.

**Supporting functionality**

- Subscribe to shows and browse episodes.
- Add an episode to a chosen queue.
- Playback with per-episode resume position.
- AI-assisted queue routing: an opt-in setting where the app _suggests_ which queue a newly added episode belongs in. Suggestion only—you still place it.

## Data Model

| Entity          | Purpose                                       |
| --------------- | --------------------------------------------- |
| `Show`          | Catalog metadata from the provider            |
| `Episode`       | Feed content, unique guid per show            |
| `PlaybackState` | Global progress — one per episode, all queues |
| `Queue`         | User-created container; auto-remove-played    |
| `QueueEntry`    | Ordering and position within a queue          |
| `QueueCursor`   | The per-queue independence mechanism          |

The load-bearing decision: progress is **global** (same episode, same position everywhere) while order and cursor are **per-queue**. That keeps the independence promise without duplicating listening history.

## Success Metric

**Weekly Multi-Queue Listeners** — users who play episodes from two or more distinct queues within seven days. It's a deliberately unforgiving metric: it only moves when people actually use the differentiator, not when they just press play.

## Milestones

- **M0 — Foundation:** subscribe, browse, catalog integration.
- **M1 — Queues:** full CRUD, ordering, per-queue cursors.
- **M2 — Playback:** AVPlayer, background audio, durable resume.
- **M3 — Semantic search:** embedding pipeline, vector search, keyword fallback.
- **M4 — AI routing and onboarding:** queue suggestions, opt-in setting, two-interest startup.
- **M5 — Launch readiness:** analytics, error handling, App Store submission.

M1 and M2 together form a shippable product on their own; M3 and M4 add the distinctive parts.

## Technology Stack

- **Platform:** Native iOS 17+, Swift, SwiftUI
- **Persistence:** SwiftData, local-first, transactional queue writes
- **Audio:** AVPlayer with MPNowPlayingInfoCenter for native controls and background playback
- **Catalog:** Podcast Index API with an iTunes fallback
- **Search:** Supabase + pgvector for vector ANN search, with keyword fallback offline
- **AI routing:** Embedding-based nearest-centroid clustering — no LLM calls, reusing episode embeddings
- **Analytics:** Local-first event logging

Search runs over a bounded corpus—subscribed shows plus a curated discovery set—to hold infrastructure cost under a hard ceiling. If discovery feels thin, the response is to narrow the claim rather than blow the budget.

## Out of Scope for v1

Android, web, Apple Watch, cross-device sync, accounts, cloud backup, social features, per-queue playback settings, queue sharing, transcript-level search, and fully automatic queue generation. Cross-device sync is the leading candidate for post-launch reconsideration.

## Outcome

_In active development against the milestone plan above._
