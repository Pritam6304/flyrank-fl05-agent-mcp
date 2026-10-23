# FL-05 — Agent Concepts and MCP Basics

## Explainer

### Workflow versus agent

An AI **workflow** is a fixed program around one or more model calls. The developer decides the order of steps, the handoff between them, and usually the stopping condition before the user runs it. The model may produce important content inside each step, but it is not choosing the overall route. An **agent** is different because the model directs the process at runtime. It decides which tool to use next, interprets the result, changes its plan when the environment disagrees, and stops when it believes the task is complete or a guardrail is reached. In Anthropic’s wording, workflows use predefined code paths, while agents let the language model dynamically control its process and tool use [1].

That distinction is about control flow, not how impressive the prompt sounds. A long chain of prompts can still be a workflow if every step and branch is predetermined. Conversely, an agent can be a relatively small loop: give the model tools, let it act, feed back the tool result as ground truth, and repeat until a success condition, human checkpoint, or iteration limit is met. Agents are useful for open-ended jobs where the number and order of steps cannot be known in advance, but they cost more and can compound mistakes, so sandboxing and limits matter [1].

### What FL-04 is today

My FL-04 build, “Ship an Automation Workflow v2,” is a **workflow**, not an agent. Its requested pipeline is deliberately staged: gather source material, synthesize it, draft the output, review it, and format the final result. The handoffs are defined before execution, and the same kind of input follows the same sequence. Claude, NotebookLM, or another model may do the intellectual work inside a stage, but the surrounding process does not ask the model to invent a new route. That is a good design for a repeatable research or writing task because it is easier to test, explain, time, and review.

### What MCP adds

The **Model Context Protocol (MCP)** is an open standard for connecting an AI application to external systems. The official documentation compares it to USB-C: the client and the external system can interoperate through a common interface instead of requiring a one-off integration for every pair [2]. MCP does not magically make a model an agent. It gives the model a consistent way to reach information and actions that are outside the chat context.

MCP exposes three useful primitives. **Tools** are callable operations, such as reading a file, querying a service, searching a database, or creating an item. They can change state, so a client should show the call and apply permissions. **Resources** are addressable context, such as a document, a database record, or a live page that the client can retrieve and place into the model’s context. **Prompts** are reusable, parameterized instructions supplied by the server; they help a client offer a reliable task template without hiding the underlying instructions. In practice, a server might expose a `search_sources` tool, a `source://...` resource containing the result, and a `write_brief` prompt that tells the model how to turn evidence into a cited brief.

For this submission I used the Manus MCP client against live browser and page-reading tools. The three evidence tasks were: open the FL-04 assignment page, read the Anthropic agent essay, and read the official MCP introduction. Each task returned external page content through a tool call; none could have been completed from the chat message alone. The captured evidence is in [`evidence.md`](evidence.md).

### Turning FL-04 into an agent

The concrete upgrade is an **evidence-seeking research agent**. Instead of forcing every topic through the same gather → synthesize route, the model would receive tools for searching sources, opening a result, extracting a passage, checking a citation, and saving a draft. It would first inspect the brief, decide how many sources and what kinds of sources are needed, then choose the next search or reading action based on what it has found. If two sources conflict, it would seek a third source or ask for a human decision. If evidence is thin, it would not draft confidently; it would continue researching or report the gap.

The agent would still need explicit guardrails: an allowed-domain list, a source-count minimum, citation checks, a maximum number of tool turns, and a human approval checkpoint before publishing. Those controls preserve the useful parts of FL-04—clear outputs and review—while making the route adaptive. The key change is not adding more prompts. It is giving the model responsibility for selecting and revising the next action based on live tool results.

## References

[1]: https://www.anthropic.com/research/building-effective-agents "Building effective AI agents — Anthropic Engineering"
[2]: https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro "What is the Model Context Protocol? — Model Context Protocol"
[3]: https://internship.flyrank.ai/intern/assignments/FL-04 "FlyRank FL-04 — Ship an Automation Workflow v2"
[4]: https://internship.flyrank.ai/intern/assignments/FL-05 "FlyRank FL-05 — Agent Concepts and MCP Basics"
