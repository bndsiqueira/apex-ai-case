# AI agents

Apex AI has three agents, each with a different audience.

| Agent | Talks to | Job |
| --- | --- | --- |
| Pre-sales agent | The dealership's customers, on WhatsApp | Greets, finds the right department, qualifies, books visits, collects financing forms and hands off |
| Support agent | People who use Apex AI | Answers product questions and helps them get unstuck |
| Sales manager agent | Store managers, through reports | Reads real conversations between salespeople and leads, and reports on response time and dropped leads |

A store can run several pre-sales agents, each with its own persona and number. There are types for dealerships, used-vehicle stores and general businesses, and the type decides what the agent is allowed to do.

## Handing off to a person

The hardest part of a sales agent is knowing when to stop. When the agent hands off, it picks the owner, writes a summary, moves the lead to the right stage and applies a tag. The salesperson starts with context.

We also delay lead ownership on numbers with an AI. A lead has no owner while the agent is working, and the agent assigns one at handoff. A safety net assigns an owner if the conversation goes quiet, so nobody falls through.

## Keeping behavior reliable

**Fix the engine, not the store's prompt.** Stores customize their prompts. When an audit of more than 1,200 conversations found the same mistakes across stores, we stopped patching prompts one by one. Behavior rules that apply everywhere live in the engine, and a store's prompt can't turn them off.

**Keep the simulator honest.** Stores test their agent in a simulator. Tests check that the simulator and the production engine apply the same rules. Otherwise the simulator approves what production gets wrong.

**Follow up with rules.** If a customer goes quiet, the agent follows a schedule the store configures. It stops when a person joins the conversation, and it only writes during service hours.

## Operating the product through MCP

Apex AI has an MCP server, so an AI assistant can operate the admin panel in plain language: read a conversation, test an agent, change a catalog item.

Each connector has an expiry date, a scope and a way to revoke it. Reads and routine changes go straight through. Creating or deleting an agent becomes a request that a person approves inside the product.

We added expiry and scopes after a connector ran a test tool in a loop and spent real API money. An AI client doesn't get tired, so limits have to live on the server.

## Why the sales manager agent runs outside the product

Reading long conversations every day is expensive with per-token billing. So that agent runs outside Apex AI and works through MCP. Inside the product, plain SQL calculates the numbers, and the product stores the finished reports.
