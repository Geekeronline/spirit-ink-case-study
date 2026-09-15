# Technical troubleshooting

These are four integration issues I investigated during the project. In each case, the visible problem appeared in one part of the system while the cause was somewhere else in the flow.

## 1. A later workflow stage could not see a status update

A record was meant to move through two stages in sequence: generate its mockup, then build the store product. The first stage completed and the database showed the new status, but the second stage still did not run.

I checked the filter and the database write first. Both were correct. The issue was how the Make router handled data inside the same execution: both routes were evaluating the version of the record captured when the execution started.

I separated the two stages into different executions and used the existing approval step as the boundary. That allowed the second stage to read the updated state normally.

## 2. An unsupported expression produced empty output

Admin review cards appeared blank or only showed part of the expected summary even though the underlying data was present.

The data itself was fine. Two text builders used `concat()`, which was not supported in that Make expression context. Because the failure did not surface as a useful save-time error, it looked like missing data further downstream.

I replaced the expression with Make's native interpolation and checked the live workflow again to make sure it was still active and had no incomplete executions waiting.

## 3. Shopify added parameters that the image endpoint rejected

Product preview images disappeared during one part of the School Store review flow, while the same images still rendered correctly when called directly.

Comparing the two request paths showed that requests passing through Shopify included additional transport parameters. The downstream image endpoint rejected parameters it did not expect.

The proxy now validates the incoming request and forwards only the parameters the image endpoint needs. That restored the previews without weakening the request checks.

## 4. A supplier API rejected signed requests

Supplier order requests kept returning an invalid-signature error even though the payload and secret had already been checked.

I compared the request construction with the supplier documentation and found that the implementation did not match the signing method the API expected.

After correcting the request to match the documented method, the supplier accepted it and returned an order identifier.

## What I took from these cases

The most useful checks were usually at the boundaries between systems: when Make reads state, what Shopify adds to a request, or exactly what a supplier expects from an API call. Testing those assumptions directly was more effective than changing the component closest to the visible symptom.
