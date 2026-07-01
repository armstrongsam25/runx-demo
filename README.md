# runx Demo: Governed Agent Skill Workflows

This repo is a hands-on walkthrough of [runx](https://github.com/runxhq/runx) — the governed runtime for agent skill workflows.

## What is runx?

runx turns expertise into portable agent infrastructure. A **skill** is a `SKILL.md` file published at a URL; agents can pull it into their own environment, compose it with other skills, and build chains of useful work without bespoke glue code.

The key insight: runx admits each act under **explicit authority**, delivers credentials without turning them into prompt material, runs the declared profile, and **seals the result into a receipt**. Authority narrows through the chain, so agent work can compound without becoming ambient trust.

## Why runx Matters

Most "agent" frameworks today are just prompt engineering with extra steps. The model writes some text, maybe calls a tool, and everyone hopes it works out. There's no governed boundary between "the agent thought about doing X" and "the agent actually did X with real consequences."

runx fixes this by making every consequential action flow through a runtime that:

1. **Admits authority** — each skill declares what authority it needs (payment, data write, external API call). The runtime checks that authority exists before the skill runs.
2. **Seals receipts** — every skill execution produces a signed receipt with the inputs, outputs, authority used, and a cryptographic seal. This is verifiable proof, not a log entry.
3. **Narrows authority through chains** — a parent skill can delegate to child skills, but the children get strictly less authority than the parent. No ambient trust escalation.

## Quick Start

```bash
# Install the runx CLI
npm install -g @runxhq/cli

# Initialize a workspace
runx init

# Create a new skill
runx new my-first-skill

# Run a skill from the public registry
runx skill armstrongsam25/hello-world --json

# Verify a receipt
runx verify --receipt receipt.json --json
```

## What a Skill Looks Like

A runx skill is a directory with:

- `SKILL.md` — the portable skill contract (frontmatter + procedure)
- `X.yaml` — the execution profile (runner wiring, inputs, outputs, authority)
- `fixtures/*.yaml` — test cases for the harness

### Example: Hello World

```markdown
---
name: hello-world
description: A minimal skill that greets the caller and returns a receipt.
source: https://github.com/armstrongsam25/runx-demo
---

## What this skill does
Greets the caller by name and returns a sealed receipt.

## Procedure
1. Read the `name` input
2. Compose a greeting
3. Return the greeting as structured output

## Output schema
{ "greeting": "string", "recipient": "string" }
```

## The Receipt

Every runx skill execution produces a receipt. Here's what one looks like:

```json
{
  "receipt_id": "runx:receipt:abc123",
  "status": "sealed",
  "skill": "hello-world",
  "inputs": { "name": "Sam" },
  "outputs": { "greeting": "Hello, Sam!", "recipient": "Sam" },
  "authority": { "type": "none", "scope": "read-only" },
  "seal": {
    "algorithm": "ed25519",
    "kid": "runx:signing-key:1",
    "signature": "..."
  }
}
```

The receipt is the proof that the skill ran with the declared inputs and produced the declared outputs. A reviewer can verify it without trusting the agent's prose.

## MCP Serving

runx skills can be served over MCP (Model Context Protocol):

```bash
runx mcp serve hello-world --http-listen 0.0.0.0:3000
```

This exposes the skill as an MCP tool that any compatible agent (Claude, Codex, etc.) can call. The agent brings the judgment; runx admits the authority, performs the act, and returns a signed receipt.

## The Frantic Bounty Board

runx powers [Frantic](https://gofrantic.com) — a bounty board where AI agents do honest work for real money. Every claim, delivery, and judgment is sealed to a public ledger. This demo repo was created as part of a Frantic bounty submission.

## Learn More

- [runx on GitHub](https://github.com/runxhq/runx)
- [runx.ai](https://runx.ai)
- [Skill spec](https://runx.ai/SKILL.md)
- [Frantic bounty board](https://gofrantic.com)

## License

MIT
