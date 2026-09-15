# Architecture

## One owner per fact

The system crosses several services, so the most important architecture rule is that each category of truth has one authority. A convenient duplicate becomes dangerous once another component starts treating it as canonical.

| System | Owns |
| --- | --- |
| **Backend service** (Cloud Run) | Product/colour/composition identity, print geometry and production contracts |
| **Shopify** | Commerce resources, checkout and customer data, orders and fulfilment records |
| **Airtable** | Lean durable operational state: supplier mappings, runtime configuration, queues and order ledger |
| **Make** | Orchestration, state transitions and safety guards; not product or catalogue authority |

Airtable is deliberately **not** a product catalogue or allowlist. Make can enforce workflow state and safety conditions, but it does not hard-code which products, colours, sizes or suppliers exist.

## The commerce path

Five scenarios divide the commerce flow by failure domain:

1. **Product creation and mapping** — turns a finished composition into the required Shopify resources and records the exact downstream supplier mapping.
2. **Paid order-line ingestion** — takes paid Shopify order lines into durable runtime state.
3. **Production package build** — assembles the artwork references, placement geometry and composition data required for production.
4. **Supplier dispatch** — routes each line to the correct supplier and creates the supplier order only after idempotency and mapping checks pass.
5. **Tracking and fulfilment** — reconciles supplier tracking back into Shopify as a fulfilment.

Keeping these stages separate gives each one its own retry and recovery behaviour. A supplier timeout, malformed placement data and an ambiguous create-order outcome should not share the same error strategy.

## The routing identity problem

A garment can carry several identifiers that look similar but have different semantics:

- the **internal Shopify SKU** — a label used inside the commerce system
- the **supplier variant SKU** — the supplier's variant label
- the **supplier API product identifier** — the identifier accepted by the supplier API

They are not interchangeable and are never derived from one another.

The exact Shopify **ProductVariant GID** is the downstream routing identity. Human-readable labels and internal SKUs are reference data for people, not routing keys for machines. Supplier identifiers are populated only from authoritative supplier data. If an exact mapping is missing, dispatch fails closed rather than inferring one.

See [Reliability](reliability.md) for the failure behaviour around missing or ambiguous supplier data.

## Product-agnostic by constraint

Product names, IDs, colours, sizes, prices, variant counts and supplier examples are not hard-coded into the orchestration layer. Adding a product family is primarily a data/contract change rather than a new branch in Make.

The delivered catalogue covers nine product families. Front/back production is currently registry-driven for seven families, each supporting Front only, Back only, or Front + Back where the production contract allows it.

That distinction matters: the catalogue scope and the back-print registry are related, but they are not the same thing.

## School stores

Each school is a tenant represented inside one Shopify store. School-specific presentation metadata can change without changing the canonical product identity.

That avoids a common multi-tenant failure: renaming a shared Shopify product for one school would rename it everywhere. School-specific names and descriptions therefore sit in the presentation layer rather than mutating the shared commerce resource.

Store management uses authenticated Shopify customer identity through a signed App Proxy path. Draft management actions, such as rename and logical delete, re-read the school by authenticated customer identity plus stable school ID and reject incompatible states before changing anything.
