---
name: wicked-verify
description: "Fact-check what you are about to say or act on: verify claims against a source text (grounded) or live web evidence (open) and act only on supported ones. Use before sending summaries, reports, citations or anything irreversible built on facts."
version: 0.1.0
author: Wicked API
license: MIT
metadata:
  hermes:
    tags: [verification, hallucination, accuracy, research, wicked]
    category: research
---

# Verify before you assert

Use this before you deliver a summary, report, answer with citations, or take an action that rests on facts
you have not checked: sending an email that states numbers, filing something, changing config from a doc.

## Requirements

The hosted Wicked MCP server must be connected (one time, by the user):

```
hermes mcp add wicked --url "https://mcp.wickedapi.com/mcp"
```

`sanity_check` and `sanity_check_batch` need either a free Wicked Sanity key set on the server side or an x402
payment. If the call returns a payment challenge (HTTP 402), **stop and tell the user the price**; do not pay
without their yes.

## Procedure

1. **Extract the claims.** List the specific, checkable statements (numbers, dates, names, quotes, "X says Y").
   Skip opinions and anything you can verify by reading the source yourself.
2. **Pick the mode.**
   - `grounded` (default): the claims must be faithful to a source you hold, e.g. the document you summarised.
     Pass that text as `context`. This is the right mode for summaries and answers from provided material.
   - `open`: check against live web evidence. This measures consistency with the web, not objective truth. Use for
     claims with no source in hand, and say so in your report.
3. **Batch.** Send everything in one `sanity_check_batch` call (up to 50 claims) instead of one call per sentence.
4. **Apply the policy** to each verdict:
   - `supported`: keep it.
   - `unsupported` or `insufficient_evidence`: keep only if clearly labelled as unverified, or remove it.
   - `contradicted`: do not assert it. Fix it from the source and re-check the corrected claim.
5. **Report briefly**: how many claims, how many supported, and exactly which were changed or flagged.

## Rules

- A check is evidence, not proof. Never present "supported" as guaranteed truth.
- The claim text and context are sent to the Wicked Sanity service. Do not include secrets or private data; trim
  `context` to the relevant passage.
- Do not loop: re-check a corrected claim once, then report what you could not confirm.
