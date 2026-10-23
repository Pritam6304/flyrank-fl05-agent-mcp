# MCP evidence

## Client and server

**Client:** Manus agent session using the `manus-tools` MCP server.

**What was demonstrated:** the client used MCP browser tools to reach external pages and return their live contents. These are not model-only answers: the tool calls navigated to and read pages outside the chat context.

## Task 1 — Read the FL-04 pipeline

**Tool call:** `manus-tools/browser_navigate`

**Input:** `https://internship.flyrank.ai/intern/assignments/FL-04`

**Result:** The live assignment page returned the FL-04 brief, including the fixed pipeline stages: gather, synthesize, draft, review, and format. It also returned the stated no-code implementation options and evaluation criteria.

![Task 1 result: FL-04 live page](evidence/01-fl04-workflow.webp)

## Task 2 — Read the agent/workflow definition

**Tool call:** `manus-tools/browser_navigate`

**Input:** `https://www.anthropic.com/engineering/building-effective-agents`

**Result:** The live Anthropic page returned the distinction that workflows follow predefined code paths, while agents dynamically direct their own process and tool use. It also returned the guidance to use agents for open-ended tasks and to apply limits because autonomy increases cost and error risk.

![Task 2 result: Anthropic live page](evidence/02-anthropic-agents.webp)

## Task 3 — Read the MCP introduction

**Tool call:** `manus-tools/browser_navigate`

**Input:** `https://modelcontextprotocol.io/introduction`

**Result:** The live MCP documentation returned the definition of MCP as an open standard connecting AI applications to external systems, along with examples of data sources, tools, and workflows that can be connected.

![Task 3 result: MCP live page](evidence/03-mcp-introduction.webp)

## Why chat alone could not do these tasks

The three results depended on network access, page navigation, and extraction of the current content from external websites. The evidence images show the pages returned by the tool-backed browser session, while the explainer uses those results to classify FL-04 and propose an agent upgrade.
