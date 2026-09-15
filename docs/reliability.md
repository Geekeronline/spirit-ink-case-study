# Reliability

The system can spend real money and create physical fulfilment work, so the important behaviour is what happens when data is missing or the outcome of an external call is uncertain.

## Fail closed, and record why

When required supplier data is missing, a line does not dispatch. It stops with a named, machine-readable blocker describing what is unconfirmed.

That makes the failure actionable. A generic error says only that automation stopped; a precise blocker says which supplier fact must be obtained before the line can safely continue.

One Supplier B cap colour/size mapping is intentionally held on this basis. It remains fail-closed until authoritative supplier identity data exists. This is a known data limitation, not an implementation defect.

## Claim before create

Supplier dispatch uses claim-before-create semantics. If a supplier order identifier already exists for an order line, the system does not create another order.

The harder case is an ambiguous outcome: the create request left the system, but the response did not return. The supplier order may or may not exist. That state routes to manual review rather than blind retry, because retrying can create a duplicate physical order and charge the client twice.

## Humans gate customer-visible actions

Automation prepares school-store and product work up to a reviewable state. Publication remains human-approved.

The review surface therefore needs to show the thing being approved — rendered artwork and customer-facing presentation — rather than only raw field values. A gate is useful only if the person can evaluate it quickly and correctly.

## Safety rules

- Production routing acts only on exact, confirmed supplier mappings.
- Products and school stores reach customer-visible publication only after the relevant review gate.
- Credentials, webhook URLs, approval tokens and infrastructure identifiers stay out of version control and working documentation.
- Signature verification, authenticated identity, supplier mapping and idempotency checks fail closed.
- Ambiguous supplier-create outcomes stop for reconciliation instead of retrying blindly.
- Live system readback overrides stale exports or documentation when they disagree.
- Real paid supplier orders are not manufactured solely to make an end-to-end proof claim true.

## What "done" means here

The primary supplier path has natural paid-order proof through the five-stage automation: production, shipment, carrier tracking and Shopify fulfilment all completed on the real path.

The second supplier path is active for confirmed mappings. Its first natural paid order remains useful post-delivery evidence, but it is not required for the project to be considered complete. Missing supplier data continues to fail closed until the authoritative value exists.
