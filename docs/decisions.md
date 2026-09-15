# Decisions

Six choices that shaped the system. Each one includes the cost, because the trade-off is part of the decision.

---

## 1. Supplier identifiers are sourced, never inferred

**Decision.** Supplier product identifiers are populated only from authoritative supplier data. If an exact value is missing, the mapping stays incomplete and that line cannot be dispatched.

**Why.** Internal SKUs are intentionally readable, and supplier identifiers can look as though they follow the same pattern. Deriving one from the other can produce a plausible identifier that is still wrong — exactly the kind of error that can send the wrong garment into production without an obvious software exception.

**Cost.** One Supplier B cap colour/size mapping remains intentionally fail-closed because its authoritative supplier identity has not been confirmed. A guess could remove the blocker quickly; the system does not treat a guess as data.

---

## 2. Separate scenarios instead of one long pipeline

**Decision.** The commerce path is split into five orchestration scenarios.

**Why.** Each stage has a different failure model. A transient supplier timeout may be retryable; malformed production geometry needs intervention; an ambiguous create-order outcome must stop rather than retry blindly.

**Cost.** There are more moving parts, and state must be durable between stages instead of living only inside one execution.

---

## 3. Airtable is runtime state, not a catalogue

**Decision.** Airtable holds operational mappings, configuration, queues and the order ledger. It does not decide which products exist or what may be sold.

**Why.** The failure mode is gradual: a table gets one row per sellable thing for convenience; an automation starts checking for that row; later a valid product silently stops working because the table has become an accidental allowlist.

**Cost.** Some changes that could have been implemented as a quick table lookup instead require an explicit backend or contract change.

---

## 4. Human approval stays at irreversible boundaries

**Decision.** Automation prepares products and school stores for review, but customer-visible publication remains gated by a person.

**Why.** Automation makes correct work fast, but it also makes a bad assumption propagate quickly. The client is the right authority for whether school artwork and presentation are ready to publish.

**Cost.** The system is intentionally not described as fully hands-off. A short review step is retained where the cost of a wrong automated decision would reach a customer.

---

## 5. Live orchestration is patched from current state, not old exports

**Decision.** Before changing a live scenario, read its current state and patch only the required modules. Exported blueprints are evidence and backup context, not deployment authority.

**Why.** Reapplying an older stored blueprint can overwrite newer live work or alter platform metadata outside the intended change. In a production-sensitive automation, that is a larger risk than making a smaller live patch after a fresh readback.

**Cost.** Changes take longer and are less suitable for broad batch updates.

---

## 6. Front and back are independent placements

**Decision.** A composition can be Front only, Back only, or Front + Back. Each side has its own geometry, offset, scale, template and design identity. The same artwork can be used on both sides, but the placements remain independent.

**Why.** Treating back print as a boolean on a front-print product fails as soon as the artwork differs by side or the physical print areas differ. Independent placements keep those cases explicit in the production contract.

**Cost.** The model and production-package contract are larger than a simple front/back flag, and the change required a real contract migration rather than a cosmetic UI switch.

---

## What I'd revisit

The approval gate is manual by design, but the waiting queue could be instrumented better. The client can see what is pending; the next improvement would be ageing/notification around how long an item has been waiting for review.
