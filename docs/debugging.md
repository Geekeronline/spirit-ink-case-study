# Debugging

Four representative failures where the visible symptom and the actual cause lived in different parts of the system.

---

## 1. Two routes, one frozen snapshot

**Symptom.** A record was meant to pass through two stages in sequence: generate its mockup, then build the store product. Stage one ran; stage two, whose filter depended on the status just written by stage one, did not.

**Initial hypothesis.** The filter or database write was wrong. The stored status, however, was visibly correct.

**Root cause.** The orchestration router evaluated both routes against the bundle captured when the execution began. Route one updated the database, but route two was still evaluating the earlier snapshot. The filter was correct; the execution model made the new state unavailable inside that routing decision.

**Fix.** Split the stages into separate executions, with the existing human approval boundary between them.

**Takeaway.** Before changing a filter, confirm when the platform evaluates it and whether it can observe state written earlier in the same execution.

---

## 2. An unsupported expression function failed quietly

**Symptom.** Admin review cards rendered blank or showed only a fragment of the expected summary even though the underlying records contained the right values.

**Initial hypothesis.** Missing or malformed data.

**Root cause.** Two string builders used `concat()`, which was not supported in that Make expression context. The builder did not surface a useful save-time error, so the failure looked like empty data downstream.

**Fix.** Rewrote the two builders with native interpolation, then read the live scenario back and confirmed it remained active with no incomplete executions queued.

**Takeaway.** An expression layer can fail without producing the kind of exception expected from application code. Empty output is often a reason to test the template/expression engine itself.

---

## 3. Signed-proxy transport fields reached a strict renderer

**Symptom.** Product preview images disappeared at one point in the School Store review flow even though direct image rendering worked.

**Initial hypothesis.** Rendering or caching failure.

**Root cause.** Requests at that point passed through a signed Shopify App Proxy, which adds transport fields. The downstream image renderer deliberately rejects unknown parameters. The request was valid at the proxy boundary but invalid against the renderer's stricter contract.

**Fix.** Verify the signed proxy request first, then remove only the known Shopify transport fields before forwarding the image request to the canonical renderer. Unknown image parameters and modified signatures still fail closed.

**Takeaway.** When a request is enriched in transit, the adapter boundary needs an explicit contract for what is verified, what is stripped, and what must remain untouched.

---

## 4. The authentication construction was not HMAC

**Symptom.** Supplier order requests were rejected consistently as having an invalid signature even though the payload and secret were correct.

**Initial hypothesis.** Wrong secret or body serialisation.

**Root cause.** The supplier's authentication scheme used a provider-specific hash construction over the raw body and secret rather than the HMAC pattern I had assumed. The two approaches looked similar at integration level but were cryptographically different.

**Fix.** Reproduce the documented construction exactly instead of adapting the familiar HMAC pattern. Once the signing scheme matched, the API accepted the request and returned an order identifier.

**Takeaway.** Consistent authentication failure is often structural. Re-read the exact signing contract before continuing to tweak inputs that are already correct.

---

## The pattern underneath all four

In each case, the system was behaving according to a rule I had modelled incorrectly: route evaluation timing, expression support, proxy request shape, or signature construction. The useful debugging step was identifying the assumption and testing it directly rather than changing the nearest piece of code to the symptom.
