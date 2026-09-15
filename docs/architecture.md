# Architecture

## One owner per type of data

The system uses several services, so each type of data has one clear source of truth. Other systems may keep references or working copies, but they should not become competing authorities.

| System | Main responsibility |
| --- | --- |
| **Backend service** (Cloud Run) | Product, colour and composition identity; print geometry; production contracts |
| **Shopify** | Products and variants, checkout, customer data, orders and fulfilment records |
| **Airtable** | Supplier mappings, runtime configuration, queues and order state |
| **Make** | Workflow orchestration, state transitions and safety checks |

Airtable stores operational data, but it does not decide which products are allowed to exist. The Make workflows also avoid hard-coding product names, colours, sizes or supplier options.

## Commerce flow

The paid-order path is split into five stages:

1. Create the Shopify product and save the supplier mapping.
2. Bring paid Shopify order lines into the runtime state.
3. Build the production package with artwork and placement data.
4. Send each line to the correct supplier after the required checks pass.
5. Bring supplier tracking back into Shopify fulfilment.

These stages are separate because they fail in different ways. A temporary supplier timeout can be retried. Invalid print-placement data needs correction. An uncertain supplier-order creation needs manual reconciliation before anything is sent again.

## Product and supplier identifiers

The same garment can have several identifiers:

1. An internal Shopify SKU used for reference inside the store.
2. A supplier variant SKU.
3. A supplier API product identifier used when placing the supplier order.

They may look similar, but they serve different purposes.

The exact Shopify **ProductVariant GID** is used to identify the selected Shopify variant in the downstream flow. Supplier identifiers only come from confirmed supplier data. If the exact mapping is missing, the line stops instead of trying to infer one.

See [Reliability](reliability.md) for how missing and uncertain supplier data is handled.

## Product setup

Product names, IDs, colours, sizes, prices and variant counts are kept out of the Make workflow logic. Adding a new product family is mainly a catalogue and production-contract change rather than a new hard-coded branch in the automation.

The delivered catalogue covers nine product families. Seven currently support front printing, back printing or both through the production registry.

The full catalogue and the front/back print registry are separate pieces of configuration, so the two numbers are not expected to match.

## School stores

All school stores live inside the same Shopify store. A school can have its own display names and descriptions without changing the shared Shopify product itself.

This matters because the same product may appear in more than one school store. Changing the shared Shopify product name for one school would affect every other school using it.

Store management is tied to the signed-in Shopify customer through the App Proxy flow. Before a draft school is renamed or deleted, the system checks the customer identity, the school ID and the current school state.
