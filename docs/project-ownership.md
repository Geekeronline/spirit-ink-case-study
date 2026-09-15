# Project ownership

## Keeping the work coordinated

The project spans several connected services, so I kept one current source of truth for the system state and checked the live setup before making changes.

That mattered most when work continued across different sessions or touched more than one platform. If the documentation and the live system disagreed, I treated that as something to resolve before changing anything.

## Making changes safely

I tried to keep live changes narrow and reversible. Before editing a Make scenario, I checked its current state and changed only the part involved in the issue.

For higher-risk actions, such as supplier order creation or customer-facing publication, the system uses explicit checks or human review instead of assuming that an uncertain state is safe.

## Working with a non-technical client

I translated technical states into clear next actions.

For example, instead of describing a product option as "fail-closed pending authoritative data", I would explain that it could not be sold yet because the supplier had not confirmed the required identifier, then state exactly what information was needed.

I used the same approach for status updates. Built, deployed, tested and proven through a real order are different stages, so I kept those distinctions clear when reporting progress.

## Using AI during implementation

I used AI coding agents extensively as part of the implementation process. I remained responsible for the requirements, architecture decisions, constraints given to the agents, validation against the live system and final acceptance of changes.

That meant checking generated changes against the actual system rather than treating a plausible output as proof that something worked.

## Handoff

I kept the project documentation current throughout the work and prepared a final handoff that explains the live architecture, open evidence points and operating constraints without relying on old chat history or debugging notes.
