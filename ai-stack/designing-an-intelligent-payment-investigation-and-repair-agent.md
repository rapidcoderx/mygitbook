---
description: >-
  How AI agents, MCP tools, and operational prompts can transform payment
  operations
icon: user-hat-tie-magnifying-glass
---

# Designing an Intelligent Payment Investigation & Repair Agent

Payment failures are one of the most operationally intensive workflows in banking systems.

Across payment types — real-time transfers, batch payments, internal transfers, or cross-border transactions — failures can occur due to:

* validation errors
* routing issues
* compliance flags
* liquidity constraints
* transient processing failures

When a payment fails, operations teams must investigate across several systems:

* payment processing engine
* validation and routing services
* compliance screening systems
* settlement or liquidity systems
* ledger records

Even a simple question like:

> Why did this payment fail?

often requires navigating **multiple dashboards and operational tools**.

This process is:

* manual
* dependent on expert knowledge
* repetitive
* difficult to scale

Most banks attempt to address this by building **more dashboards**.

But dashboards solve **visibility**, not **investigation**.

***

## Why AI Is Actually Useful Here

Many AI use cases in banking feel forced. Payment investigation is not one of them.

Investigating a failed payment is essentially a **structured reasoning workflow**:

1. Retrieve payment status
2. Check validation errors
3. Verify compliance screening
4. Confirm settlement liquidity
5. Identify the root cause
6. Determine whether the issue can be repaired

This is a **multi-step decision workflow across systems**.

AI agents are well suited for this because they can:

* orchestrate multiple system queries
* correlate signals across systems
* reason about failure causes
* summarize findings clearly

Instead of replacing systems, the AI agent acts as an **investigator that understands how those systems interact**.

***

## From Investigation to Repair

Once the root cause is identified, many payment issues can be repaired automatically.

Examples include:

* retrying transient processing failures
* correcting missing payment fields
* selecting alternative routing
* retrying after settlement funding

Instead of simply explaining the failure, the system can suggest a **repair action**.

```
AI investigates → AI suggests repair → Human approves → System executes
```

This introduces a **human-in-the-loop automation model** that balances speed with operational safety.

***

## Why Not Just Build Another Dashboard?

Dashboards assume operators already know:

* which system to check
* how to interpret errors
* how systems relate to each other

Modern banking platforms are **distributed architectures** with multiple services interacting.

Dashboards display **data fragments**.

AI agents instead:

* retrieve information dynamically
* correlate signals across systems
* explain the root cause

The difference is important.

Dashboard → **information display**

Agent → **problem investigation**

***

## Why MCP Instead of Direct API Access?

One might ask:

> Why not simply give the AI direct API access?

In regulated financial environments this introduces risks:

* uncontrolled system interactions
* inconsistent API usage
* limited governance
* lack of auditability

Model Context Protocol (MCP) introduces a **controlled capability layer** between AI agents and enterprise systems.

Instead of exposing raw APIs, MCP exposes **structured tools**.

Benefits include:

* permission-controlled capabilities
* schema validation
* auditable tool usage
* safer integration boundaries

MCP becomes the **interface layer between AI agents and banking systems**.

***

## Architecture Overview



```mermaid
%%{init: {
  "theme": "forest",
  "themeVariables": {
    "fontFamily": "Inter, Segoe UI, Roboto, sans-serif",
    "fontSize": "24px",
    "fontWeight": "1000",
    "actorFontSize": "24px",
    "sequenceNumberFontSize": "24px"
  }
}}%%

flowchart TD
    %% --- Styles ---
    classDef human fill:#fcefe3,stroke:#e0a46b,stroke-width:2px,color:#5a3d1e;
    classDef agent fill:#e8f4ff,stroke:#6aa3d8,stroke-width:2px,color:#1b3c59;
    classDef llm fill:#eef7ff,stroke:#7aa6d9,stroke-width:2px,color:#1a2f4b;
    classDef tool fill:#f0f7f0,stroke:#7bb47b,stroke-width:2px,color:#1f3d1f;
    classDef system fill:#fff8e6,stroke:#d4b15a,stroke-width:2px,color:#5a4a1e;
    classDef decision fill:#fdecef,stroke:#d46a7e,stroke-width:2px,color:#5a1e2a;
    classDef action fill:#e8fff3,stroke:#6bcf9b,stroke-width:2px,color:#1e4d36;

    %% --- Roles & Initiation ---
    A[👤 Operations Analyst<br/>Initiates Case] --> B[🔍 Payment Investigation Agent]
    class A,B human

    %% --- Cognitive Reasoning Loop ---
    B --> C[🧠 Reasoning LLM<br/>Contextual Analysis & Hypothesis Generation]
    class C llm

    C --> D[🔧 MCP Tool Layer<br/>Action Orchestration]
    class D tool

    %% --- System Integrations ---
    D --> E1[💳 Payment Processing System<br/>Transaction Status & Routing]
    D --> E2[🛡️ Validation Services<br/>Format, Rules & Sanity Checks]
    D --> E3[⚖️ Compliance Screening<br/>Sanctions, AML, KYC]
    D --> E4[🏦 Settlement & Liquidity Systems<br/>Funding & Position Checks]
    D --> E5[📘 Payment Ledger<br/>Authoritative Record]

    class E1,E2,E3,E4,E5 system

    %% --- Feedback Loop ---
    E1 --> D
    E2 --> D
    E3 --> D
    E4 --> D
    E5 --> D
    D --> C
    C --> B

    %% --- Recommendation & Governance ---
    B --> F[🛠️ Repair Recommendation<br/>Fix, Re-route, Request Info, or Reject]
    class F agent

    F --> G{🧑‍⚖️ Human Approval<br/>Risk & Policy Oversight}
    class G decision

    %% --- Outcomes ---
    G -->|Approved| H[⚙️ Execute Repair Tool<br/>Automated Fix Applied]
    G -->|Rejected| I[📈 Escalate Investigation<br/>Senior Analyst or Specialist]

    class H action
    class I human

```

This architecture introduces a **capability layer** that allows AI agents to safely interact with banking infrastructure.

***

## MCP Tools for the Payment Agent

Instead of calling APIs directly, the agent interacts with **domain-specific tools**.

#### Payment Trace MCP

```
trace_payment(payment_reference)
get_payment_status(payment_reference)
get_payment_route(payment_reference)
```

Connected systems:

* payment processing engine
* routing services
* payment ledger

***

#### Payment Validation MCP

```
get_validation_errors(payment_reference)
validate_payment_fields(payment_reference)
```

Connected systems:

* validation services
* payment gateway

***

#### Compliance MCP

```
check_screening_status(payment_reference)
get_compliance_flags(payment_reference)
```

Connected systems:

* AML monitoring
* compliance screening systems

***

#### Settlement & Liquidity MCP

```
get_settlement_balance(account_id)
check_intraday_liquidity(entity_id)
```

Connected systems:

* settlement accounts
* liquidity management systems

***

#### Repair Execution MCP

```
retry_payment(payment_reference)
repair_payment_fields(payment_reference)
route_payment_alternative(payment_reference)
```

Repair tools execute **only after human approval**.

***

## Agent Model Architecture

An effective payment investigation agent consists of three main components.

1. **Reasoning LLM**

The core model performs:

* investigation reasoning
* tool selection
* root-cause analysis

The model must support:

* multi-step reasoning
* function / tool calling
* structured outputs

***

2. **MCP Tool Layer**

MCP exposes safe operational capabilities.

The model decides **which tool to call**, and MCP executes the request.

***

3. **Human Control Layer**

All repair actions require **human approval**.

This ensures:

* operational oversight
* regulatory compliance
* auditability

***

## Agent Internal Architecture

```mermaid
%%{init: {
  "theme": "forest",
  "themeVariables": {
    "fontFamily": "Inter, Segoe UI, Roboto, sans-serif",
    "fontSize": "24px",
    "fontWeight": "1000",
    "actorFontSize": "24px",
    "sequenceNumberFontSize": "24px"
  }
}}%%

flowchart LR

    %% === STYLE DEFINITIONS ===
    classDef user fill:#fcefe3,stroke:#e0a46b,stroke-width:2px,color:#5a3d1e;
    classDef agent fill:#e8f4ff,stroke:#6aa3d8,stroke-width:2px,color:#1b3c59;
    classDef llm fill:#eef7ff,stroke:#7aa6d9,stroke-width:2px,color:#1a2f4b;
    classDef tool fill:#f0f7f0,stroke:#7bb47b,stroke-width:2px,color:#1f3d1f;
    classDef system fill:#fff8e6,stroke:#d4b15a,stroke-width:2px,color:#5a4a1e;
    classDef decision fill:#fdecef,stroke:#d46a7e,stroke-width:2px,color:#5a1e2a;
    classDef action fill:#e8fff3,stroke:#6bcf9b,stroke-width:2px,color:#1e4d36;

    %% === FLOW ===
    A[🙋 User Query<br/>Intent, Issue, or Payment Error] --> B[🧭 Agent Controller<br/>Routing & Context Setup]
    class A user
    class B agent

    B --> C[🧠 Reasoning LLM<br/>Deep Analysis & Hypothesis Formation]
    class C llm

    C --> D[🎯 Tool Selection<br/>Determine Required MCP Tools]
    class D tool

    D --> E[🔧 MCP Tool Call<br/>Execute API / System Actions]
    class E tool

    E --> F[🏦 Banking Systems<br/>Core Processing, Validation, Ledger]
    class F system

    %% Feedback loops
    F --> E
    E --> C

    %% Downstream reasoning
    C --> G[🔎 Root Cause Analysis<br/>Identify Failure Mode]
    class G agent

    G --> H[🛠️ Repair Recommendation<br/>Fix, Re-route, Request Info]
    class H agent

    H --> I{🧑‍⚖️ Human Approval Gate<br/>Risk & Policy Oversight}
    class I decision

    I -->|Approved| J[⚙️ Repair Tool Execution<br/>Automated Fix Applied]
    class J action

    I -->|Rejected| K[📈 Manual Investigation<br/>Escalation to Specialist]
    class K user

```

This architecture separates **reasoning, tools, and control**.

***

## Agent Prompt Design

The intelligence of the system is not only in the model but also in the **operational prompt**.

Example system prompt:

```
You are a Payment Investigation and Repair Agent for banking operations.

Your goal is to determine the root cause of failed or delayed payments
and recommend the safest repair action.

Rules:
- Use MCP tools to gather evidence
- Do not guess system state
- Verify validation, compliance, and liquidity conditions
- Only recommend repair actions that are safe and reversible
- Always require human approval before execution

Investigation sequence:
1. Check payment status
2. Check validation errors
3. Check compliance screening
4. Check settlement liquidity
5. Determine root cause
6. Recommend repair if possible

Return:
- root cause
- supporting evidence
- confidence level
- recommended action
- human approval requirement
```

This prompt effectively encodes the **investigation playbook used by payment operations teams**.

***

## Investigation Workflow

```mermaid
%%{init: {
  "theme": "forest",
  "themeVariables": {
    "fontFamily": "Inter, Segoe UI, Roboto, sans-serif",
    "fontSize": "24px",
    "fontWeight": "1000",
    "actorFontSize": "24px",
    "sequenceNumberFontSize": "24px"
  }
}}%%

sequenceDiagram
    autonumber

    participant Ops as 👤 Operations Analyst
    participant Agent as 🤖 Payment Agent<br/>LLM + Controller
    participant MCP as 🔧 MCP Tools<br/>API Orchestration
    participant Systems as 🏦 Banking Systems<br/>Core Services

    Ops->>Agent: Why did payment REF‑827261 fail?

    Agent->>MCP: trace_payment()
    MCP->>Systems: Query payment processor<br/>Status, routing, timestamps
    Systems-->>MCP: Payment status response

    Agent->>MCP: get_validation_errors()
    MCP->>Systems: Validation service<br/>Format, rules, schema checks
    Systems-->>MCP: Validation results

    Agent->>MCP: check_screening_status()
    MCP->>Systems: Compliance screening<br/>Sanctions, AML, KYC
    Systems-->>MCP: Screening result

    Agent->>MCP: check_intraday_liquidity()
    MCP->>Systems: Liquidity system query<br/>Funding & settlement positions
    Systems-->>MCP: Liquidity & balance info

    Agent-->>Ops: 📘 Root cause identified<br/>🛠️ Recommended repair action

```

***

## The Architectural Shift

Traditional operations workflow:

```
Human → Dashboards → Multiple Systems
```

Agent-driven workflow:

```
Human → AI Agent → MCP Tools → Banking Systems
```

The underlying banking systems remain the same.

Payment engines still process transactions.\
Compliance systems still screen payments.\
Liquidity platforms still manage settlement.

What changes is the **interface to those systems**.

Instead of operators manually navigating dashboards and correlating information across platforms, an intelligent agent performs the investigation — gathering evidence, explaining the root cause, and recommending the safest repair action.

In other words, the architecture does not replace banking systems.

It adds a **reasoning layer above them.**

And that reasoning layer may ultimately prove more valuable than simply adding another dashboard.
