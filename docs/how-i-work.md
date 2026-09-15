# How I work

## AI-assisted implementation, with explicit ownership

A substantial part of this project was implemented with AI coding agents. I disclose that because the useful question is not whether code was generated; it is who owned the architecture, validation and production outcome.

My role was to maintain the system model and constraints while using agents for implementation: decide which system owns each fact, review proposed changes against those boundaries, verify live state before edits, reproduce failures, and reject outputs that were plausible but unsupported by authoritative data.

For example, an agent can easily derive a supplier-looking identifier from an internal SKU. The result can look consistent and still be unsafe. The important control is that supplier identity is never inferred: it must come from authoritative supplier data or remain blocked.

## Written handoff as infrastructure

Agent sessions end and parallel work can drift, so the project used a written coordination layer with a few operating rules:

- **Read current state before changing anything.** Live systems and current code beat an old conversation or exported blueprint.
- **One write owner per connected system at a time.** Parallel edits to the same live automation are an operational risk, not a normal merge conflict.
- **Treat contradictions as information.** If documentation and the running system disagree, stop and resolve the difference instead of silently choosing whichever version is convenient.
- **Edit the current section instead of appending another current-state block.** Version history holds the past; the working document should remain readable.
- **Keep secrets and operational identifiers out of documentation.** The case study was easier to publish because sensitive values were excluded from the working narrative from the start.

## Working with a non-technical client

Technical accuracy is only useful if the person making the business decision can act on it.

Instead of saying that a supplier mapping is "fail-closed pending authoritative data", the client-facing version is: "we cannot sell this option yet because the supplier has not confirmed the code; here is the exact information we need from them."

The same rule applies to status reporting. Built, deployed, validated and naturally proven are different states. I keep those distinctions explicit even when a looser description would sound more impressive.

## Where I focus

My current focus is integration, automation and production-system ownership: systems that span several services, have real operational consequences, and need clear failure behaviour.

I am also building deeper software-engineering fundamentals in Python and data tooling. I treat that as ongoing development rather than blurring the boundary between the work I can own confidently today and the areas where I am still increasing depth.
