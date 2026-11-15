# API

Rules for HTTP and GraphQL APIs that agents call directly or through a generated CLI or MCP server. The [shared principles](../principles/agent-as-reader.md) apply as well.

## Errors

1. **Use [RFC 9457 Problem Details](https://www.rfc-editor.org/rfc/rfc9457).** `type`, `title`, `status`, `detail`, `instance`, plus extension fields for `retryable`, field-level `errors` and a `hint`. The `type` URI lets an agent look up docs programmatically. Details in [errors](../principles/errors.md).
2. **Separate retryable from not.** Rate limits and transient failures say so and pass through `Retry-After`. Validation and auth failures say don't.

## Responses

3. **Field selection.** A `fields` parameter, or GraphQL's native equivalent. Overfetch costs the agent tokens, not you. See [output size](../principles/output-size.md).
4. **Pagination with sane defaults,** and `total` plus `next_cursor` in the response.
5. **Definitive empties.** `[]` with `total: 0`.

## Writes

6. **Idempotency keys on create-like operations,** because agents double-submit.
7. **Preconditions.** ETags or revision numbers, so a stale agent can't overwrite newer work. See [safe mutations](../principles/safe-mutations.md).
8. **A dry-run mode** that validates server-side and returns the diff.

## Contract

9. **The spec is the source.** Generate the SDK, the CLI and the MCP tools from the OpenAPI or GraphQL schema. Hand-written wrappers drift.
10. **A machine-readable spec at a stable URL,** reachable without auth.
11. **Write descriptions for a model.** Parameter and operation descriptions are what an agent reads to pick the right call. Two operations with similar names and vague descriptions are where wrong-tool errors come from.

## MCP next to the API

[Naftiko argues](https://naftiko.io/blog/put-mcp-next-to-your-api-not-on-top/) against stacking an MCP server on top of the API, one tool per endpoint. The costs are an extra hop, a second auth and deploy surface, and a second contract that drifts. Their alternative is to treat REST and MCP as sibling adapters over the same flow. It's a vendor post, and the claim that MCP becomes the main interface is opinion. The cost argument for a single source of truth holds either way. See [mcp/](../mcp/README.md).

## GraphQL

A typed, discoverable schema suits agents. One operation can replace several tool calls, and the agent doesn't need a tool per endpoint. [The layered approach on magiroux.com](https://magiroux.com/posts/from-use-case-to-graphql-operation) goes from cheap to expensive:

1. Skills and local operations
2. A persisted-operation index searched for the closest match
3. Agentic operation building against the schema
4. LLM-proposed schema changes through review

His open problem is a cheap fit check before an agent commits to a layer. Misses are costly.

## Checking

[Zuplo's agent-readiness checklist](https://zuplo.com/learning-center/api-readiness-gap-agent-callable-apis) and [API7's guide](https://api7.ai/blog/designing-apis-for-agentic-clis) are reasonable starting lists. Neither has a conformance suite.
