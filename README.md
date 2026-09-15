<a href="https://spirit-ink.co.uk/">
  <img src="assets/spirit-ink-banner.png" alt="Spirit Ink" width="100%">
</a>

# Spirit Ink automation case study

Spirit Ink is an e-commerce automation project for custom-printed school clothing.

Customers create a design in the browser and buy through Shopify. After payment, the automation prepares the order for production, sends it to the correct print supplier and brings tracking back into Shopify.

On the technical side, I was responsible for the system design, the integrations between services, production safeguards, technical troubleshooting and project handoff. The agreed scope was completed on **15 September 2026**.

## The problem

Spirit Ink works with schools and PTAs that want to sell branded clothing.

Without automation, the work includes designing graphics, collecting artwork, setting up products, preparing print files, keeping track of supplier variants, sending orders to the right supplier and updating Shopify when they ship. The workload grows with every school and product option.

The goal was to automate as much of that process as possible while still stopping when important supplier data was missing or uncertain.

## What the system does

```mermaid
flowchart LR
  A[Create a design] --> B[Create product in Shopify]
  B --> C[Customer checks out]
  C --> D[Check supplier mapping]
  D --> E[Prepare print files]
  E --> F[Send order to supplier]
  F --> G[Receive tracking]
  G --> H[Update Shopify]

  S[School account] --> T[Create or manage school store]
  T --> A
```

The customer stays in the Spirit Ink and Shopify flow. Once an order is paid, the automation handles the production steps in the background.

Schools also have a separate store-management flow. They can build a school range, keep it as a draft, submit it for review and manage it later from the same customer account.

The system covers nine product families across two print suppliers. Seven currently support front printing, back printing or both. The main supplier flow has completed a real paid order from Shopify checkout through shipment, tracking and fulfilment. The second supplier is live for mappings that have been fully verified.

## My role

I defined how the services should connect and where each type of data should live.

I was responsible for the integrations between Shopify, Make, Airtable, Google Cloud Run, Cloudinary and the two print supplier APIs.

I investigated issues that crossed more than one system, including Shopify request handling, Make workflow behaviour, supplier data and fulfilment edge cases.

For the higher-risk parts of the flow, I added checks to prevent duplicate supplier orders, incorrect product routing and unintended customer-facing publication.

I also kept the project documentation current and prepared the final handoff so another person could understand the live setup without reconstructing it from old tests or incident notes.

## Read next

| Document | What's in it |
| --- | --- |
| [Architecture](docs/architecture.md) | How the main components fit together and where each type of data lives |
| [Decisions](docs/decisions.md) | Technical decisions and the tradeoffs behind them |
| [Technical troubleshooting](docs/troubleshooting.md) | Examples of cross-system issues I investigated |
| [Reliability](docs/reliability.md) | Checks around supplier routing, duplicate actions and unsafe retries |
| [Project ownership](docs/project-ownership.md) | How I coordinated the work, validated changes and handled handoff |

## Stack

Shopify, Make, Airtable, Google Cloud Run, Cloudinary, GitHub and two print-on-demand supplier APIs.
