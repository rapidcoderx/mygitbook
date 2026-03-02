---
description: One Tool. Any Domain. — How MCP Changes the Way AI Connects to Core Banking
icon: mcp
---

# Building MCP on Banking APIs

### What is MCP — and Why Should You Care?

Most conversations about AI agents focus on the model — how smart it is, how fast it responds. Very few talk about **how the AI actually connects to real systems**.

That's where **Model Context Protocol (MCP)** comes in.

MCP is an open standard that defines a universal contract between an AI agent and the tools it uses. Instead of writing custom integration code every time you connect an AI to a new service, MCP gives you a structured, reusable way to expose capabilities — and any AI host that speaks MCP can discover and use them automatically.

Think of it this way:

> _REST is how your frontend talks to your backend. MCP is how your AI agent talks to your services._

Three things make it genuinely powerful:

* **Discoverability** — The agent asks the MCP server what's available at runtime. No hardcoded function lists.
* **Portability** — Build once, connect to Claude, Cursor, or any MCP-compatible host without changing your integration.
* **Intent-first design** — Tools are described in plain language. The AI reads the description to decide when to use a tool — making the _description itself_ the intelligence layer.

***

### The Core Banking Challenge

Core banking systems are rich with domain APIs — deposits, loans, payments, KYC, customer data. Connecting an AI agent to these the traditional way means writing bespoke code for every API, handling authentication separately per service, and rebuilding everything when APIs change.

The result: brittle integrations that break often, can't be reused, and take weeks to extend.

#### Without MCP

The AI agent talks directly to each API — one custom handler per endpoint, no shared auth, no reusability. Adding a new product or connecting a different AI tool means starting from scratch.

```
AI Agent ──(custom glue)──▶ Account API
         ──(custom glue)──▶ Loan API
         ──(custom glue)──▶ Payment API
         ──(custom glue)──▶ KYC API
```

Every arrow is a separate integration. Every integration is a liability.

#### With MCP

The AI agent talks to **one thing** — the MCP server. The server handles routing, authentication, and spec management internally. The agent just says what it wants to do.

```
AI Agent ──(MCP Protocol)──▶ MCP Server ──▶ Retail Deposits API
                                        ──▶ Retail Loans API
                                        ──▶ Corporate Deposits API
                                        ──▶ Corporate Loans API
```

One connection. All domains.

***

### The Architecture: Consumer × Domain

The key design decision was **not** to model tools around business operations — that leads to an explosion of one-off tool definitions that can't be reused.

Instead, the tool surface is structured around two axes:

| Consumer Type | Domain   | Sub-Products                                      |
| ------------- | -------- | ------------------------------------------------- |
| **Retail**    | Deposits | Savings · Fixed Deposit · Recurring Deposit       |
| **Retail**    | Loans    | Home Loan · Personal Loan · Auto Loan · Gold Loan |
| **Corporate** | Deposits | Current Account · Overdraft · Fixed Deposit       |
| **Corporate** | Loans    | Term Loan · Working Capital · Commercial Credit   |

This means the entire banking surface is covered by a **single generic tool**:

```
banking_execute(consumer, domain, operation, payload)
```

* `consumer` — `retail` or `corporate`
* `domain` — `deposits` or `loans`
* `operation` — the specific action, drawn from the OpenAPI spec
* `payload` — the request data for that operation

The AI agent receives a description of this tool in plain language, picks the right parameters based on context, and calls it. The MCP server does the rest — routing, authentication, and response formatting.

***

### Resources: The Server Describes Itself

Beyond the tool, the MCP server exposes the full **OpenAPI spec for each domain as a Resource**.

This means an AI agent can _read the spec at runtime_ — understanding available operations, required fields, and response shapes — before it even makes a tool call. The server is fully self-describing.

| Resource URI                        | What It Exposes                                              |
| ----------------------------------- | ------------------------------------------------------------ |
| `banking://retail/deposits/spec`    | Full OpenAPI spec for retail deposit operations              |
| `banking://retail/loans/spec`       | Full OpenAPI spec for home, personal, and other retail loans |
| `banking://corporate/deposits/spec` | Corporate current accounts, OD, and fixed deposit spec       |
| `banking://corporate/loans/spec`    | Term loans, working capital, and commercial credit spec      |

No documentation hunting. No hardcoded operation lists. The agent reads the spec, understands the domain, and makes informed tool calls.

***

### Mock Data — No Live API Needed

Development and testing don't require a live banking environment. Each domain has a **Faker-powered mock layer** that generates schema-consistent, realistic data on demand.

For **Retail Loans**, a mock response for a home loan query looks like this:

**Request:**

```
consumer: retail
domain:   loans
operation: getLoanDetails
payload:  { loanAccountId: "HL-2023-7821", includeEMI: true }
```

**Mock Response:**

```
Loan ID:       HL-2023-7821
Product:       Home Loan
Sanctioned:    ₹50,00,000
Outstanding:   ₹42,10,500
EMI Amount:    ₹42,800
Next Due:      2025-04-05
Status:        ACTIVE
```

For a **Personal Loan** query:

**Request:**

```
consumer: retail
domain:   loans
operation: getPersonalLoanSummary
payload:  { customerId: "CUST-4492" }
```

**Mock Response:**

```
Loan ID:       PL-2024-3341
Product:       Personal Loan
Disbursed:     ₹3,00,000
Outstanding:   ₹1,85,400
Interest Rate: 13.5%
Tenure:        36 months
Status:        REGULAR
```

The mock layer uses the same schema as the real API — so switching from mock to live is a config flag, not a code change.

***

### Drop Any Spec → Instantly MCP-Ready

The most underrated property of this architecture: **it's spec-agnostic**.

Because the tool is generic and routing is driven by the OpenAPI spec, adding a new banking product is as simple as dropping its spec file into the right folder. The server auto-discovers it on the next start, registers the resource, and the AI can immediately query it through the existing `banking_execute` tool.

No new tool definitions. No code changes. Just the spec.

> _Trade Finance tomorrow? Drop in `corporate/trade-finance.openapi.json`. Done._

***

### Auth: Two Strategies, One Layer

Authentication is handled centrally in a middleware layer — completely separate from tool logic. The strategy is chosen per environment:

* **API Key** — for sandbox and development. Simple, fast, no token lifecycle to manage.
* **OAuth2** — for production, where operations are performed on behalf of a real user with delegated identity.

The tool never knows which auth strategy is active. That separation means moving from sandbox to production is a configuration change, not a refactor.

***

### Key Takeaways

1. **One tool covers everything** — `banking_execute(consumer, domain, operation, payload)` is the only tool the AI needs. Consumer type and domain are just parameters, not separate tools.
2. **The spec drives everything** — Operations, routing, and resource exposure are all derived from the OpenAPI spec. The server has no hardcoded business logic.
3. **Self-describing via Resources** — The AI can read the live OpenAPI spec before calling a tool. This makes multi-step agent workflows dramatically more reliable.
4. **Mock first, real API later** — Faker-powered mocks match the real schema exactly. Development moves fast without needing infrastructure.
5. **Truly generic** — Any new banking domain or product becomes MCP-ready the moment its OpenAPI spec is available. This framework doesn't need to know what the product is.

***

### What Changed

Before this week, I thought of APIs as _endpoints to call_. Now I think of them as _capabilities to describe_. That shift — from request-oriented to intent-oriented thinking — is what MCP forces you to make. And once you've made it, you can't go back.

***

_Full implementation details, tool schemas, and the spec-loader pattern are documented in the linked repository._

_Curious — what's the hardest part of connecting LLMs to your existing systems? Drop a comment below._

***

**Tags:** `#MCP` `#AIAgents` `#CoreBanking` `#OpenAPI` `#FinancialServices` `#NodeJS` `#WeeklyLearning #`AI
