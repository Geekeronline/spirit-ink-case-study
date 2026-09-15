# Spirit Ink — automation case study

A production e-commerce system for custom-printed school apparel. A customer designs a garment in the browser, checks out on Shopify, and the order reaches the correct print supplier before tracking returns to Shopify as a fulfilment — without manual re-keying between systems.

I was the technical owner for the project: architecture, integrations, debugging, production safeguards, and client-facing documentation. The agreed scope was completed and delivered on **15 September 2026**.

> **This repository is a written case study, not the production source code.** The production repository is private. Credentials, infrastructure identifiers, webhook URLs, supplier identities, operational IDs and commercial terms are deliberately excluded.

---

## The problem

Schools and PTAs sell branded apparel to raise funds. Setting up a school store manually means collecting artwork, producing mockups, creating Shopify products and variants, mapping each sellable option to the right supplier, and then moving paid orders and tracking information between systems.

That work scales with every school and every product option. It also fails badly when identifiers are confused: a value that looks plausible can still route the wrong garment to a supplier.

The design goal was therefore simple: **automate the path, but refuse to guess when authoritative data is missing.**

## What the system does

```mermaid
flowchart LR
  MP["Magic Preview<br/>design + composition"] --> M0["SI-MP-00<br/>product + mapping"]
  M0 --> SHOP["Shopify<br/>checkout"]
  SHOP --> M1["SI-MP-01<br/>paid order-line"]
  M1 --> M2["SI-MP-02<br/>production package"]
  M2 --> M3["SI-MP-03<br/>supplier dispatch"]
  M3 --> SUP["Print supplier"]
  SUP --> M4["SI-MP-04<br/>tracking"]
  M4 --> F["Shopify<br/>fulfilment"]

  SS["SI-SS-01<br/>School Store provisioning"] --> SC["School collection<br/>+ owner account"]
  SC --> MP
```

Six automation scenarios support the delivered system. Five form the commerce and fulfilment path; one handles School Store provisioning and authenticated account-management actions.

The catalogue spans **nine product families** across two print suppliers. The current front/back production registry supports **seven** of those families with Front only, Back only, or Front + Back compositions.

The primary supplier path has natural paid-order end-to-end proof through production, shipment, carrier tracking and Shopify fulfilment. The second supplier path is live for confirmed mappings; its first natural paid order remains useful production evidence, but it is not an unfinished project requirement.

## What I was responsible for

- **Architecture** — assigning a single authority for each category of data and keeping product/catalogue authority out of the orchestration layer.
- **Integrations** — Shopify Admin GraphQL, a signed Shopify App Proxy, two supplier APIs, Airtable runtime state, Cloudinary assets, and a backend service on Google Cloud Run.
- **Debugging** — reproducing and root-causing failures that crossed browser, orchestration, backend, Shopify and supplier boundaries.
- **Safety design** — idempotency, fail-closed supplier routing, strict signature handling, and human approval gates for irreversible or customer-visible actions.
- **Documentation and client communication** — maintaining an operational source of truth, recording decisions as they changed, and handing off the delivered system in a form another operator could continue from.

## Read next

| Document | What's in it |
| --- | --- |
| [Architecture](docs/architecture.md) | The components, boundaries, and authority model |
| [Decisions](docs/decisions.md) | Six choices that shaped the system, including their costs |
| [Debugging](docs/debugging.md) | Four representative integration failures and their root causes |
| [Reliability](docs/reliability.md) | Fail-closed behaviour, idempotency and human gates |
| [How I work](docs/how-i-work.md) | AI-assisted implementation and the operating discipline around it |

## Stack

Shopify (Admin GraphQL, App Proxy, Liquid theme) · Make · Airtable · Google Cloud Run · Cloudinary · two print-on-demand supplier APIs · GitHub

## Confidentiality

This repository is intentionally publishable. It contains no credentials, private webhook URLs, internal base/table/record/connection/scenario IDs, theme IDs, supplier names, customer data, contract terms or client pricing.

The project name is retained for the case study; operational details that are not needed to evaluate the work are not.
