---
id: BDR_005
type: Business Decision Record
source_agent: Agent 1 (Business Catalyst)
---
# Business Decision Record: BDR_005 — Human-Centric Expert Assistant Model

## 1. Description
This record defines and restricts the operational interaction paradigm between the Human-in-the-Loop (HITL) and the AI agent ecosystem. It establishes that all agents function strictly as 1:1 specialized assistants to the human, completely eliminating autonomous horizontal agent negotiation or alignment loops.

## 2. Core Principles: The Interaction Paradigm
* **The 1:1 Partnership:** Every agent operates exclusively as an expert craftsman or draftsman paired with the human. Agents exist to ingest human intent, surface knowledge gaps, and model structured proposals based on direct human instruction.
* **Vertical Communication Flow:** 
    * The interaction pattern is strictly vertical (Human $\leftrightarrow$ Agent). 
    * The human works with Agent 2 (PM) to model requirements (`PREQ`). 
    * The human explicitly transitions that output to Agent 3 (Architect).
    * The human works with Agent 3 to model the technical solution (`TDR`). 
* **Inter-Agent Isolation:** Agents are strictly forbidden from communicating autonomously behind closed doors to resolve system, logic, or design issues. 
* **Conflict Surfacing Mandate:** If two agents cross paths or surface contradictory logic, they cannot negotiate a compromise. They must present the conflict and any alternative solution vectors directly to the human as a structured choice.

## 3. Business Justification
* **Absolute Alignment:** Prevents agentic drift, recursive looping overhead, and out-of-context technical decisions.
* **Total Human Sovereignty:** Reaffirms that all architectural, structural, and business decisions originate from or are explicitly approved by the human director, keeping the system completely traceable.

## 4. Operational Enforcement
* **Instruction Constraint:** The system prompts for all 7 agents must explicitly contain this 1:1 vertical interaction mandate.
* **Rejection of Autonomy:** Any execution phase where an agent attempts to invoke or delegate tasks to another agent without explicit human intervention shall fail validation during the QA audit stage.