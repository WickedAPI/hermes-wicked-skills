# Wicked Skills for Hermes Agent

Two small, instruction-only skills that make a Hermes agent more careful. No code runs; they teach the agent a
procedure and use the free tools of the hosted [Wicked MCP server](https://mcp.wickedapi.com).

| Skill | What it does |
|---|---|
| `wicked-vet` | Before installing, calling or paying a third-party tool, MCP server or agent: look up its real reliability score and the owner's reputation, then give a go / caution / stop verdict. |
| `wicked-verify` | Before asserting or acting on facts: batch-check claims against your source text or live web evidence and keep only what is supported. |

## Install

Connect the Wicked MCP server once:

```
hermes mcp add wicked --url "https://mcp.wickedapi.com/mcp"
```

Add this repo as a skill source and install:

```
hermes skills tap add OWNER/hermes-wicked-skills
hermes skills install wicked-vet
hermes skills install wicked-verify
```

(Replace `OWNER` with the GitHub owner of this repo.)

## Disclosures

- Skills are plain Markdown. They contain no scripts and read no files or credentials.
- When used, the agent calls Wicked MCP tools, which send the URL, name or claim text the agent chooses to the Wicked
  services. The skills tell the agent never to include secrets.
- Registry/reputation lookups used are free. Anything priced returns a payment challenge, and the skills require the
  user's yes before any spend.

Companion: [hermes-wicked-memory](https://github.com/choaticpixels/hermes-wicked-memory) (long-term memory plugin).

License: MIT.
