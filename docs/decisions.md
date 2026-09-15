# Decisions

These are the main technical choices that shaped the system and the tradeoffs that came with them.

## 1. Do not guess supplier identifiers

Supplier product identifiers only come from confirmed supplier data. If an exact value is missing, that mapping stays incomplete and the order line cannot be sent to production.

Internal SKUs are readable and supplier codes can look similar, which makes guessing tempting. A plausible-looking identifier can still point to the wrong product.

**Tradeoff:** any product option without a confirmed supplier identifier stays unavailable until it is verified.

## 2. Keep the commerce stages separate

The paid-order flow is split across five Make scenarios instead of one long workflow.

Each stage has a different recovery path. A supplier timeout may be safe to retry. Invalid production data needs correction. An uncertain create-order result needs reconciliation before another request is sent.

**Tradeoff:** there are more workflows to maintain, and state has to persist between them.

## 3. Use Airtable for runtime state

Airtable holds supplier mappings, runtime configuration, queues and order state. Product availability is controlled elsewhere.

This avoids turning a convenient operational table into an accidental product allowlist. A valid product should not stop working simply because someone forgot to add a row to Airtable.

**Tradeoff:** some changes need an explicit backend or contract update instead of a quick table edit.

## 4. Keep human approval before publication

The automation can prepare products and school stores for review, but a person still approves customer-facing publication.

That review point is useful because artwork and presentation are easier for a person to judge than for an automation. It also limits the impact of a bad assumption before customers can see it.

**Tradeoff:** the process includes a short manual step.

## 5. Read the live Make scenario before changing it

Before editing a live scenario, I read its current state and change only the modules involved in the issue. Older exported blueprints are kept as reference material.

Re-importing an old blueprint can overwrite newer live changes or alter parts of the scenario that were not meant to change.

**Tradeoff:** this is slower than applying broad updates from an old export.

## 6. Treat front and back as separate placements

A product can be Front only, Back only, or Front + Back. Each side has its own artwork, geometry, offset, scale and template.

This is necessary when the front and back use different artwork or different physical print areas. A single front/back flag would not carry enough information for production.

**Tradeoff:** the composition model and production package are more complex than a simple boolean setting.

## What I would improve next

The approval queue could be easier to monitor. The client can already see what is waiting for review, but ageing and notifications would make it clearer when something has been sitting there for too long.
