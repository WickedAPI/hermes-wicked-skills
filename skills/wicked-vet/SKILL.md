---
name: wicked-vet
description: "Vet a third-party tool, MCP server, paid API or another agent before you install it, call it or pay it: check its real uptime and reliability score and the other agent's on-chain reputation, then report a clear go / caution / stop."
version: 0.1.0
author: Wicked API
license: MIT
metadata:
  hermes:
    tags: [security, mcp, x402, reputation, supply-chain, wicked]
    category: security
---

# Vet before you trust

Use this when you are about to do something that depends on a third party you have no history with:
install or connect an MCP server, call a paid (x402) endpoint, send money to an address, or trust the output of
another agent. It costs a few free lookups and gives the user a decision they can act on.

## Requirements

The hosted Wicked MCP server must be connected (one time, by the user):

```
hermes mcp add wicked --url "https://mcp.wickedapi.com/mcp"
```

Every lookup used below is free and needs no key. If the `registry_*` tools are not listed, tell the user how to
connect the server and stop. Do not guess scores.

## Procedure

1. **Identify the target.** Get its name and its exact endpoint URL (or the agent's `0x` wallet). If the user only
   gave a name, ask for the URL; a name alone can match a look-alike.
2. **Look it up.** Call `registry_lookup` with `endpoint` set to the exact URL. If nothing matches, retry once with
   `q` set to the name or domain. Read the returned id and headline score.
3. **Go deeper only if it matters** (money will move, or the tool gets broad access). Call `registry_tool_detail`
   with the id. It needs a free Registry key or an x402 payment; if neither is configured, say so and use the
   headline score only. Never spend money to vet without the user's yes.
4. **Check the counterparty** when the target is an agent or has an owner wallet: `reputation_status` with the
   wallet. A 404 means never registered: treat as tier "unranked", which is neutral information, not an accusation.
5. **Give a verdict**, in this shape:

   - **Go**: found in the registry, healthy score, no red flags. Say what you checked.
   - **Caution**: not found, thin history, or middling score. Say what is missing and what limits would make it safe
     (read-only access, small amount, test call first).
   - **Stop**: poor score, repeated downtime or schema violations, or the URL does not match the claimed project.

## Rules

- "Not in the registry" is not "bad". New tools are unscored. Say unknown, not unsafe.
- A good score measures uptime, latency and schema conformance. It does **not** prove the tool is honest about
  what it does with your data. Say that when the decision involves secrets or private data.
- Never paste secrets, keys or the user's private data into any lookup. Only URLs, names and public wallets.
- If the user confirms a tool misbehaved, offer `registry_report_tool` (reason: `downtime`, `incorrect_response`,
  `scam_or_abuse`, `schema_violation` or `other`). Filing is public: show the user the report text and get a yes first.
