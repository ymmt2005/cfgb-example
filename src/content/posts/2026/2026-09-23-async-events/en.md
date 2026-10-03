---
title: Retries and event processing
slug: retries-and-events
publishedAt: '2026-09-23T08:00:00+09:00'
topics:
- software-engineering
summary: A fictional event-processing design separates delivery attempts from application effects. It uses idempotency keys, explicit retry decisions, and an audit trail to make duplicate deliveries easier to reason about.
---
## Delivery and effects

This fictional design treats an event delivery as an attempt, not proof that a business effect happened once. An idempotency key connects repeated deliveries to the same intended effect.

### Retry decisions

Record whether a failure is transient, permanent, or unknown. A retry should reuse the original event identity.

## Audit trail

An audit trail records the decision and its inputs. This is a discussion example, not a claim about any specific queue product.
