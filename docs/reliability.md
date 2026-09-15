# Reliability

This system can spend real money and create physical supplier orders, so missing data and uncertain API outcomes need to be handled carefully.

## Stop when supplier data is missing

If the system does not have the exact supplier data it needs, the order line stops before dispatch. The blocker records what is missing so the issue can be resolved without guessing.

Any mapping without a confirmed supplier identifier stays unavailable until it is verified.

## Avoid duplicate supplier orders

Before creating a supplier order, the system checks whether that order line already has a supplier order ID. If it does, no new order is created.

A harder case is when the request was sent but the response never came back. The supplier may have created the order even though the automation cannot confirm it. Those cases go to manual review before any retry, because sending the same order again could create a duplicate and charge the client twice.

## Keep publication under human review

The automation prepares school stores and products up to a reviewable state. A person approves them before they become customer-facing.

The review page shows the rendered artwork and customer-facing presentation, not just database fields, so the person approving it can see what customers will see.

## Safety rules

1. Supplier routing only uses exact, confirmed mappings.
2. Products and school stores are reviewed before customer-facing publication.
3. Credentials, webhook URLs, approval tokens and infrastructure identifiers stay out of version control.
4. Signature, identity, supplier-mapping and duplicate-order checks stop the flow when they fail.
5. Uncertain supplier-order creation goes to manual reconciliation before retry.
6. When documentation and the live system disagree, the live system is checked again before making a change.
7. Real supplier orders are not created just to produce an end-to-end test result.

## Production proof

The main supplier path has completed a real paid order through production, shipment, carrier tracking and Shopify fulfilment.

The second supplier path is active for confirmed mappings, with routing and supplier integration verified. Its first natural paid order on that path has not yet occurred.
