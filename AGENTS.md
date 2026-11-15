# agent-experience-design

A generic knowledge base on designing CLIs, APIs and MCP servers for AI agents (AX). Markdown only, no code, no build.

## Layout

- `README.md`: premise, index, the short version
- `principles/`: rules shared by every surface
- `cli/`, `api/`, `mcp/`: one `README.md` each, surface-specific rules
- `counter-position.md`: the case against rewriting
- `evidence.md`: benchmarks and how far to trust them
- `references.md`: original sources, grouped by what they support

## Adding or changing a note

1. Shared rules go in `principles/`. Surface-specific rules go in `cli/`, `api/` or `mcp/`.
2. Link a new note from the README index.
3. Add each new source to `references.md` under the matching heading.
4. Add each new measurement to `evidence.md` with who ran it and on what.

## Content rules

- Every claim with a number links to its primary source. Don't cite a newsletter or aggregator when the original exists.
- Label benchmarks as vendor-run or project-run when they are.
- Mark sources known only from an abstract or a summary as *not read in full* in `references.md`.
- Generic AX knowledge. Impersonal voice, no first person, nothing specific to one organisation.
- Describe current positions only. No changelog, no history.
- Plain sentences, sentence-case headings, no em dashes, no filler.
- Relative Markdown links between notes. No wiki-style links.

## Checks

Links are checked weekly and on pull requests by CI. Locally:

```
lychee --exclude '^https://x\.com/' --accept 200,429 '**/*.md'
```
